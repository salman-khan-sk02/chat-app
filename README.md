# chat-app

step 1 create users

the fist ste is to create users

you can create a user using the folowing API

API:-

POST http://localhost:3000/users

Requset Signature:-

{
"firstName": "Lewis",
"lastName": "Hamilton",
"type": "consumer"
}

you can create 2 or 3 users

step 2

GET http://localhost:3000/users

this API helps you get all users. once we have the responce we can get the required User ID

step 3 Login

POST http://localhost:3000/login/<userid>

ie http://localhost:3000/login/8708df0a97514dae8ddb779d7940b7f9

this will return a token which we require to use the chat APIs.
{
"success": true,
"authorization": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOiI4NzA4ZGYwYTk3NTE0ZGFlOGRkYjc3OWQ3OTQwYjdmOSIsInVzZXJUeXBlIjoiY29uc3VtZXIiLCJpYXQiOjE2NjAxNDc2MTJ9.elBZIrfk-X6M2We2ZwhSATtwm9SdmYwonpR9G2RGTPA"
}

step 4

now we initiate the Chat by creating a room with the recipient

use the the "authorization" token from the previous response in the header as a Bearer token

API:-
POST http://localhost:3000/room/initiate

Requset Signature:-
{
"userIds": ["8708df0a97514dae8ddb779d7940b7f9"],
"type": "consumer-to-consumer"
}

response:-

{
"success": true,
"chatRoom": {
"isNew": true,
"message": "creating a new chatroom",
"chatRoomId": "5531fdad28d44ca3b613ccc2f1042144",
"type": "consumer-to-consumer"
}
}

pick the "chatRoomId".

step 5 sending a message

now we send a message

use the the "authorization" token from the login response in the header as a Bearer token

http://localhost:3000/room/<roomId>/message

i.e. http://localhost:3000/room/5531fdad28d44ca3b613ccc2f1042144/message

request signature:-

{
"messageText": "hi, this is a test message"
}

response:-
{
"success": true,
"post": {
"\_id": "5531fdad28d44ca3b613ccc2f1042144",
"postId": "4cc0642538c44f808de483a6aac0a436",
"chatRoomId": "5531fdad28d44ca3b613ccc2f1042144",
"message": {
"messageText": "hi, this is a test message"
},
"type": "text",
"postedByUser": {
"\_id": "165e4b42b14e45bfaaac8f8412227c2a",
"firstName": "Lewis",
"lastName": "Hamilton",
"type": "consumer",
"createdAt": "2022-08-10T14:37:56.361Z",
"updatedAt": "2022-08-10T14:37:56.361Z",
"**v": 0
},
"readByRecipients": [
{
"readByUserId": "165e4b42b14e45bfaaac8f8412227c2a",
"readAt": "2022-08-10T16:09:10.821Z"
}
],
"chatRoomInfo": [
{
"\_id": "8708df0a97514dae8ddb779d7940b7f9",
"firstName": "John",
"lastName": "Doe",
"type": "consumer",
"createdAt": "2022-08-10T12:07:25.755Z",
"updatedAt": "2022-08-10T12:07:25.755Z",
"**v": 0
},
{
"\_id": "165e4b42b14e45bfaaac8f8412227c2a",
"firstName": "Lewis",
"lastName": "Hamilton",
"type": "consumer",
"createdAt": "2022-08-10T14:37:56.361Z",
"updatedAt": "2022-08-10T14:37:56.361Z",
"\_\_v": 0
}
],
"createdAt": "2022-08-10T16:11:16.015Z",
"updatedAt": "2022-08-10T16:11:16.015Z"
}
}

step 6

reading the meassage

we first login using the recipient id (see step 3)

next we call the http://localhost:3000/room/<roomID>/<messageID>/mark-read-delete

this API marks and deletes a message once it has been read by the recipient

API:-
http://localhost:3000/room/5531fdad28d44ca3b613ccc2f1042144/4cc0642538c44f808de483a6aac0a436/mark-read-delete

Responce:-
{
"success": true,
"data": {
"acknowledged": true,
"modifiedCount": 1,
"upsertedId": null,
"upsertedCount": 0,
"matchedCount": 1
},
"deletedMessagesCount": 1
}

step 7

you can use the followin API to view/ observe a room.

http://localhost:3000/room/<roomID>?limit=5&page=0

API:-
http://localhost:3000/room/5531fdad28d44ca3b613ccc2f1042144?limit=5&page=0

response:-
{
"success": true,
"conversation": [
{
"\_id": "92c4540faec64a51b5e9be5014ab4b9c",
"chatRoomId": "5531fdad28d44ca3b613ccc2f1042144",
"message": {
"messageText": "hi, wassup"
},
"postedByUser": {
"\_id": "165e4b42b14e45bfaaac8f8412227c2a",
"firstName": "Lewis",
"lastName": "Hamilton",
"type": "consumer",
"createdAt": "2022-08-10T14:37:56.361Z",
"updatedAt": "2022-08-10T14:37:56.361Z",
"**v": 0
},
"readByRecipients": [
{
"readByUserId": "165e4b42b14e45bfaaac8f8412227c2a",
"readAt": "2022-08-10T16:00:20.022Z"
},
{
"readByUserId": "8708df0a97514dae8ddb779d7940b7f9",
"readAt": "2022-08-10T16:03:13.815Z"
}
],
"type": "text",
"createdAt": "2022-08-10T16:02:09.214Z",
"updatedAt": "2022-08-10T16:07:12.984Z",
"**v": 0
},
{
"\_id": "4cc0642538c44f808de483a6aac0a436",
"chatRoomId": "5531fdad28d44ca3b613ccc2f1042144",
"message": {
"messageText": "hi, wassup hellooooo"
},
"postedByUser": {
"\_id": "165e4b42b14e45bfaaac8f8412227c2a",
"firstName": "Lewis",
"lastName": "Hamilton",
"type": "consumer",
"createdAt": "2022-08-10T14:37:56.361Z",
"updatedAt": "2022-08-10T14:37:56.361Z",
"**v": 0
},
"readByRecipients": [
{
"readByUserId": "165e4b42b14e45bfaaac8f8412227c2a",
"readAt": "2022-08-10T16:09:10.821Z"
}
],
"type": "text",
"createdAt": "2022-08-10T16:11:16.015Z",
"updatedAt": "2022-08-10T16:11:16.015Z",
"**v": 0
}
],
"users": [
{
"_id": "165e4b42b14e45bfaaac8f8412227c2a",
"firstName": "Lewis",
"lastName": "Hamilton",
"type": "consumer",
"createdAt": "2022-08-10T14:37:56.361Z",
"updatedAt": "2022-08-10T14:37:56.361Z",
"__v": 0
},
{
"_id": "8708df0a97514dae8ddb779d7940b7f9",
"firstName": "John",
"lastName": "Doe",
"type": "consumer",
"createdAt": "2022-08-10T12:07:25.755Z",
"updatedAt": "2022-08-10T12:07:25.755Z",
"__v": 0
}
]
}
