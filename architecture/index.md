# WebTrit Architecture

![WebTrit system architecture diagram](diagrams/WebTritSystem_Architecture.png)

Exported from [draw.io diagram](diagrams/WebTritSystem_Architecture.drawio) to PNG with `Zoom: 200%` and `Border Width: 10`.
<!-- /Applications/draw.io.app/Contents/MacOS/draw.io -x -f png -s 2 -b 10 -o WebTritSystem_Architecture.png WebTritSystem_Architecture.drawio -->

WebTrit system consists of the following components:

* Core - central system component (implemented on [Elixir](https://elixir-lang.org) based on [Phoenix](https://phoenixframework.org)) with the following functionality:
  * calls business logic
  * client's signaling protocol
  * client's API
  * admin and monitoring API
  * controlling WebRTC Server
  * collecting WebRTC Server events
  * storing necessary data in the Database
  * sending push notifications via Push Notification Server
  * retrieving necessary data from PortaBilling
* Subscriber Data Adapter Server - auxiliary system component to connect with external systems (as example [PortaSwitch adapter](https://github.com/WebTrit/webtrit_adapter_portaswitch))
* Database - [PostgreSQL](https://www.postgresql.org/)
* WebRTC Server - [Janus](https://janus.conf.meetecho.com/)
* Push Notification Server - [gorush](https://github.com/appleboy/gorush)

![WebTrit system sequence diagram](diagrams/WebTritSystem_Sequence.png)

## Deployment

![WebTrit system deployment diagram](diagrams/WebTritSystem_Deployment.png)

Exported from [draw.io diagram](diagrams/WebTritSystem_Deployment.drawio) to PNG with `Zoom: 200%` and `Border Width: 10`.
<!-- /Applications/draw.io.app/Contents/MacOS/draw.io -x -f png -s 2 -b 10 -o WebTritSystem_Deployment.png WebTritSystem_Deployment.drawio -->
