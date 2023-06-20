---
sidebar:
  nav: "docs"
title : WebRTC Client Applcation
---

WebRTC의 Client 단을 구현해보자.    
단순히 MDN Web Docs를 가져온 겁니다.      

해당 내용은 1:1 P2P Call(화상전화) 관련된 내용 입니다.

---

# 사이트

[https://developer.mozilla.org/ko/docs/Web/API/WebRTC_API/Signaling_and_video_calling](https://developer.mozilla.org/ko/docs/Web/API/WebRTC_API/Signaling_and_video_calling)

---

# HTML

```s
<div class="flexChild" id="camera-container">
    <div class="camera-box">
        <video id="received_video" autoplay></video>
        <video id="local_video" autoplay muted></video>
        <button id="hangup-button" onclick="hangUpCall();" disabled>
        Hang Up
        </button>
    </div>
</div>
```

비디오 태그 1개는 로컬 미디어 정보를 틀어주고    
다른 비디오 태그는 다른 피어에서 보내온 미디어 정보를 틀어줍니다.   

**<i class="fa fa-info-circle" aria-hidden="true"></i> 정보**   
**autoplay** : 비디오가 도달하기 시작하면 즉시 재생시키는 역할   
**muted** : 로컬 오디오를 음소거   
{: .notice--info}   

---

# Javascript Code   

#### 시그널링 서버로 메시지 보내기   

```javascript
function sendToServer(msg) {
  var msgJSON = JSON.stringify(msg);

  connection.send(msgJSON);
}
```

WebSocket Connection을 이용해서 메세지를 보내주는 코드

#### 사용자 리스트    

```javascript
function handleUserlistMsg(msg) {
  var i;

  var listElem = document.getElementById("userlistbox");

  while (listElem.firstChild) {
    listElem.removeChild(listElem.firstChild);
  }

  // …

  for (i=0; i < msg.users.length; i++) {
    var item = document.createElement("li");
    item.appendChild(document.createTextNode(msg.users[i]));
    item.addEventListener("click", invite, false);

    listElem.appendChild(item);
  }
}
```

사용자 리스트 UI부분을 만들어주는 코드   

#### 화상채팅 시작      
```javascript
var mediaConstraints = {
  audio: true, // We want an audio track
  video: true // ...and we want a video track
};

function invite(evt) {
  if (myPeerConnection) {
    alert("You can't start a call because you already have one open!");
  } else {
    var clickedUsername = evt.target.textContent;

    if (clickedUsername === myUsername) {
      alert("I'm afraid I can't let you talk to yourself. That would be weird.");
      return;
    }

    targetUsername = clickedUsername;

    createPeerConnection();

    navigator.mediaDevices.getUserMedia(mediaConstraints)
    .then(function(localStream) {
      document.getElementById("local_video").srcObject = localStream;
      myPeerConnection.addStream(localStream);
    })
    .catch(handleGetUserMediaError);
  }
}
```

P2P로 상대방과 연결 start   
PeerConnection을 생성하고, 로컬 미디어 스트림을 Peer에 추가   

#### Peer Connection 생성   

```javascript

function createPeerConnection() {
  myPeerConnection = new RTCPeerConnection({
      iceServers: [     // Information about ICE servers - Use your own!
        {
          urls: "turn:[TURN/STUN server 호스트 넣어주자]",  // 내가 만든 턴서버를 넣어주자
          username: "[사용자명]",
          credential: "[비밀번호]"
        }
      ]
  });
}
// …

```
    
PeerConnection을 생성할때 STUN/TURN 서버 urls를 넣어주자.   
없다면 서버를 만들어 줍시다 😊

#### 이벤트 핸들러 세팅   

RTCPeerConnection이 생성되면, 이벤트 handler를 설정

```javascript
// …
  myPeerConnection.onicecandidate = handleICECandidateEvent;
  myPeerConnection.onaddstream = handleAddStreamEvent;
  myPeerConnection.onremovestream = handleRemoveStreamEvent;
  myPeerConnection.oniceconnectionstatechange = handleICEConnectionStateChangeEvent;
  myPeerConnection.onicegatheringstatechange = handleICEGatheringStateChangeEvent;
  myPeerConnection.onsignalingstatechange = handleSignalingStateChangeEvent;
  myPeerConnection.onnegotiationneeded = handleNegotiationNeededEvent;
```

`onicecandidate` : 로컬에 Offer(SDP)를 setDesripction을 성공한 이후에   
ICE layer가 Event를 보냅니다.

`onaddstream` : remote stream이 추가될 때   
로컬 WebRTC layer가 Event를 보냅니다.

`onremovestream` : Connection에서 remote stream을 제거할 때 불려집니다.    

`oniceconnectionstatechange` : ICE Connection의 상태 변경을 알리기 위해
ICE layer가 Event를 보냅니다.

`onicegatheringstatechange` : ICE Agent의 candidate 수집 프로세스 상태가 변경 됐을 때
(예를들어, candiate를 모으기 시작하거나 negotiation이 끝났을 때)
ICE layer가 Event를 보냅니다.

`onsignalstatechange` : 시그널링 프로세스의 state가 바뀔 때, 
WebRTC layer가 Event를 보냅니다.

`onnegotitationneeded` : WebRTC layer가 session negotiation 프로세스를 새로 시작할 때마다
불립니다.

#### 협상 시작   

Caller가 자신의 RTCPeerConnection과 media stream을 생성하고 
Connection에 stream을 추가하게 되면   
negotiationneeded event 가 호출됩니다.

```javascript
function handleNegotiationNeededEvent() {
  myPeerConnection.createOffer().then(function(offer) {
    return myPeerConnection.setLocalDescription(offer);
  })
  .then(function() {
    sendToServer({
      name: myUsername,
      target: targetUsername,
      type: "video-offer",
      sdp: myPeerConnection.localDescription
    });
  })
  .catch(reportError);
}
```

SDP Offer를 생성하고, setLocalDescription에 설정합니다    
상대방 Peer에게 Offer를 전달합니다      
setLocalDescription이 완료되면, ICE agent가 icecandidate event들을 처리하기 시작한다.    

#### Session 협상    

다른 피어는 우리의 offer를 받을 것이고, handleVideoOfferMsg()에 전달됩니다.     

자기 자신의 RTCPeerConnection과 media stream을 생성해야 한다.   
받은 offer를 분석하고 이에 대한 answer를 만들어 보내야한다.   

```javascript
function handleVideoOfferMsg(msg) {
  var localStream = null;

  targetUsername = msg.name;

  createPeerConnection();

  var desc = new RTCSessionDescription(msg.sdp);

  myPeerConnection.setRemoteDescription(desc).then(function () {
    return navigator.mediaDevices.getUserMedia(mediaConstraints);
  })
  .then(function(stream) {
    localStream = stream;

    document.getElementById("local_video").srcObject = localStream;
    return myPeerConnection.addStream(localStream);
  })
  .then(function() {
    return myPeerConnection.createAnswer();
  })
  .then(function(answer) {
    return myPeerConnection.setLocalDescription(answer);
  })
  .then(function() {
    var msg = {
      name: myUsername,
      target: targetUsername,
      type: "video-answer",
      sdp: myPeerConnection.localDescription
    };

    sendToServer(msg);
  })
  .catch(handleGetUserMediaError);
}

// …
```

callee도 마찬가지로 setLocalDescription 실행되면, 브라우저는 callee가 반드시 처리해야하는 icecandidateevent들을 처리하기 시작한다.

#### ICE Candiates 보내기
caller가 callee로부터 answer를 받습니다.  
그리고 뒷단에서 각 피어들의 ICE agent들이 열심히 ICE candidate message들을 교환합니다.   
Peer 사이의 미디어를 어떻게 주고 받을지 알기 전까지 계속해서 candidate들을 보냅니다.   
candiate들은 시그널링 서버를 통해서 전송 되어야 합니다.   

```javascript
function handleICECandidateEvent(event) {
  if (event.candidate) {
    sendToServer({
      type: "new-ice-candidate",
      target: targetUsername,
      candidate: event.candidate
    });
  }
}
```

**<i class="fa fa-info-circle" aria-hidden="true"></i> 정보**   
Call의 다른 피어로부터 ICE candidate가 도착할 때, icecandidateevent가 전송되는 것이 아님을 항상 명심해라. 대신에 너 자신이 call을 할 때 보내는 것으로, 너가 원하는 채널을 통해 data를 보낼 수 있다. WebRTC를 처음 접한다면 매우 헷갈릴 것이다.
{: .notice--info}

#### ICE Candidates 받기
시그널링 서버를 통해서 ICE candiate가 상대방 Peer로 보내집니다.

```javascript
function handleNewICECandidateMsg(msg) {
  var candidate = new RTCIceCandidate(msg.candidate);

  myPeerConnection.addIceCandidate(candidate)
    .catch(reportError);
}
```
수신된 SDP로 RTCIceCandidate를 생성하고, 
myPeerConnection.addIceCandidate로 ICE candidate를 local ICE layer에 전달합니다.

피어들이 서로 직접 통신되는지 확인후, 합의가 이루어지면 Connection을 Open 합니다.

#### 새로운 스트림 받기
Remote 피어가 RTCPeerConnection.addStream를 부름으로써    
스트림이 커넥션에 추가되었을 때, addstreamevent가 발생합니다.

```javascript
function handleAddStreamEvent(event) {
  document.getElementById("received_video").srcObject = event.stream;
  document.getElementById("hangup-button").disabled = false;
}
```

#### 스트림 삭제

```javascript
function handleRemoveStreamEvent(event) {
  closeVideoCall();
}
```

```javascript
function hangUpCall() {
  closeVideoCall();
  sendToServer({
    name: myUsername,
    target: targetUsername,
    type: "hang-up"
  });
}
```

```javascript
function closeVideoCall() {
  var remoteVideo = document.getElementById("received_video");
  var localVideo = document.getElementById("local_video");

  if (myPeerConnection) {
    if (remoteVideo.srcObject) {
      remoteVideo.srcObject.getTracks().forEach(track => track.stop());
      remoteVideo.srcObject = null;
    }

    if (localVideo.srcObject) {
      localVideo.srcObject.getTracks().forEach(track => track.stop());
      localVideo.srcObject = null;
    }

    myPeerConnection.close();
    myPeerConnection = null;
  }

  document.getElementById("hangup-button").disabled = true;

  targetUsername = null;
}
```

stream들을 멈추고 지운 후에, RTCPeerConnectionobject를 없앤다. 


#### 상태 변화 다루기
다양한 상태 변화를 너의 코드에 알리기 위해    
listener를 세팅할 수 있는 다양한 이벤트들이 있습니다.   
```s
iceconnectionstatechange
icegatheringstatechange
signalingstatechange 
```  
  

#### ICE 연결 상태
커넥션 state가 바뀌면(예를들어, call이 다른쪽에서 중단 될 때)    
ICE layer가 iceconnectionstatechangeevent를 우리에게 보냅니다.

```javascript
function handleICEConnectionStateChangeEvent(event) {
  switch(myPeerConnection.iceConnectionState) {
    case "closed":
    case "failed":
    case "disconnected":
      closeVideoCall();
      break;
  }
}
```

ICE connection state가 "closed", 또는"failed", 또는 "disconnected"으로 바뀔 때 
closeVideoCall()함수를 실행한다.   



#### ICE 시그널링 상태   
마찬가지로 signalingstatechangeevent를 받을 수 있는데,   
시그널링 상태가 "closed"으로 바뀌면 완전히 종료시킨다. 
```javascript
myPeerConnection.onsignalingstatechange = function(event) {
switch(myPeerConnection.signalingState) {
    case "closed":
    closeVideoCall();
    break;
}
};
```

#### ICE GATHERING 상태  
ICE candidate gathering process가 변경될 때 알려주는 데 사용됩니다.    
```javascript
function handleICEGatheringStateChangeEvent(event) {
  // Our sample just logs information to console here,
  // but you can do whatever you need.
}
```