# r064o4-webrtc
Учебный проект (созданный с помощью ИИ) по созданию WebRTC для двух разных клиентов с полноценным WebSocket-сигнальным сервером на Node.js и использованием публичных Google STUN серверов.

КАК РАБОТАЕТ
При загрузке пользователь вводит свой уникальный ID и ID партнера.

Устанавливается WebSocket-соединение к сигналинговому серверу.

Настраивается RTCPeerConnection с публичными Google STUN серверами.

Локальный медиапоток с камеры захватывается и отображается.

Если id текущего пользователя лексикографически меньше id партнера, он создаёт и отправляет оффер.

Другой клиент получает оффер, отвечает ансвером.

ICE кандидаты обмениваются через WebSocket.

После установки связи в remoteVideo появляется видео другого клиента.

ЗАПУСК
Установите Node.js, создайте папку проекта.

Поместите файл server.js и выполните:

bash
npm init -y
npm install ws
node server.js
Откройте index.html и client.js через обычный HTTP-сервер (например, npx http-server), откройте страницу в двух разных вкладках или на разных устройствах.

Введите разные ID, например:

Вкладка 1: ваш ID = client1, партнер ID = client2

Вкладка 2: ваш ID = client2, партнер ID = client1

Нажмите "Начать звонок", после чего видео начнут отображаться с локальной и удаленной камеры.
*****************************************************************************************

НУЖНА ПРАВИЛЬНАЯ ПОСЛЕДОВАТЕЛЬНОСТЬ ДЕЙСТВИЙ ПРИ ПОДКЛЮЧЕНИИ!!!
1) СНАЧАЛА ПОДКЛЮЧАЕМ client2 (вводим Ваш ID: client2, ID партнера: client1 и нажимаем "начать звонок");
Увидем в консоле сообщение:
[WebRTC] WebSocket connected and registered as client2

2) ПОТОМ ПОДКЛЮЧАЕМ client1 (вводим Ваш ID: client1, ID партнера: client2 и нажимаем "начать звонок");
Увидем в консоле сообщение:
[WebRTC] WebSocket connected and registered as client1
И сразу далее побегут сообщения:
[WebRTC] Sent signaling message: {type: 'offer', from: 'client1', to: 'client2', payload: {…}},
(где payload: {sdp: 'v=0\r\no=- 5811455535006332523 2 IN IP4 127.0.0.1\r\ns…3fc8e9bc46 8d126f19-3412-4049-adeb-b11d490f5903 ...})
[WebRTC] Sent signaling message: {type: 'candidate', from: 'client1', to: 'client2', payload: RTCIceCandidate}
(где payload: RTCIceCandidate {candidate: 'candidate:4292306809 1 udp 2122260223 192.168.1.10…288 typ host generation 0 ufra ...})
[WebRTC] Sent signaling message: {type: 'candidate', from: 'client1', to: 'client2', payload: RTCIceCandidate}
(где payload: RTCIceCandidate {candidate: 'candidate:3436210189 1 udp 2122194687 172.31.0.1 53289 typ host generation 0 ufra ...}) 

...
[WebRTC] Signaling message received: {type: 'answer', from: 'client2', payload: {…}}
[WebRTC] Signaling message received: {type: 'candidate', from: 'client2', payload: {…}}
[WebRTC] Remote track received
[WebRTC] ICE state: checking
[WebRTC] ICE state: connected
[WebRTC] Signaling message received: {type: 'candidate', from: 'client2', payload: {…}}
[WebRTC] Signaling message received: {type: 'candidate', from: 'client2', payload: {…}}

На клиенте 2 побегут аналогичные записи и никаких ощибок!
Связь установлена! 