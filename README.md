# Simple WebRTC Demo

WebRTC is a peer to peer standard proposal from Google allowing browsers to connect directly and transfer information without a central server. This allows browsers to share raw data as well as audio/video.


Then open up two browser windows pointed to `localhost:3002/room_name`. 

You should see something like this: 

![screenshot1](https://www.customerly.io/dist/images/pages/live-chat-customer-support-software/Live_chat_software_video_call_customer_service.png)

# WebRTC in a nutshell

Firstly read through the article on [HTML5 Rocks](http://www.html5rocks.com/en/tutorials/webrtc/basics/). Here are the steps to create a successful connection in high-level pseudo-code:

    pc = new PeerConnection
    ws = new WebSocket

    // gets called when connection is complete
    // this is when a remote peer can stream video 
    // to your browser 
    pc.onaddstream (event) ->
      remoteVid.src = event.stream

    // local peer
    pc.createOffer (description) ->
      pc.setLocalDescription(description)
      // over websockets
      ws.send description

    ws.on 'create_offer', (data) ->
      // now this acts on a remote peer
      pc.setRemoteDescription(data)
      pc.createAnswer (description) ->
        pc.setLocalDescription(description)
        ws.send description

    ws.on 'create_answer', (data) ->
      // back on local 
      pc.setRemoteDescription(data)

    // called when handshake is complete
    pc.onicecandidate = (event) ->
      // forward to remote
      ws.send event.candidate

    ws.on 'ice_candidate', (data) ->
      pc.addIceCandidate(candidate)
    
So this song and dance is mainly complicated by the need to talk to the remote host via some transport method (websockets in this case). Check out public/javascripts/simple.js for an example of connecting two peers within the same browser window for an example of the PeerConnection API without the transport layer.

