# websocket-api-explanation

혹시 next.js 랑 관련이 없다면 알려주세요. (그럼 다른 걸 알아봐야 합니다… ㅠㅠ)

아마 kafka를 쓰고 api 컨트롤러로 바꾸면 코드가 좀 바뀌긴 할 것 같지만, 일단 기본 작동 원리는 똑같아서 적어 놓았습니디.

일단 작동하는 방식은 알았는데, api 명세서로는 어떤식으로

남겨야 하는지 몰라서 일단 적어봅니다.

직접 링크타고 가셔서 봐도 됩니다. 

[Spring STOMP 웹소켓을 이용해 채팅서비스 구현해보기(채팅방 구분, 채팅 저장)](https://blog.naver.com/PostView.naver?blogId=qjawnswkd&logNo=222283176175&parentCategoryNo=&categoryNo=&viewDate=&isShowPopularPosts=false&from=postView)

![실시간 채팅 - 1](https://user-images.githubusercontent.com/97837003/216611197-540d6c0e-d26c-4147-beca-37bc6888f230.png)


간략하게 설명을 하자면, 빨간색 부분은 웹소켓 자체에 접속하는 url 입니다.

노란색의 경우 구독을 하는 url인데, 구독을 한사람만 메세지를 받을 수 있습니다.

예를 들어 room/1 에서 메세지가 전달되면, room/2 를 구독한 사람들은 메세지를 못받고 room/1 사람들만 메세지를 전달받습니다. (’/room/’ 옆에 + 기호가 있는데 네모에 가렸네요;;)

초록색의 경우 작성한 채팅을 다시 서버로 보내는 url 입니다.

결론적으로는 색깔로 네모친 부분을 서로 맞춰야 됩니다. 

![실시간 채팅 - 2](https://user-images.githubusercontent.com/97837003/216611205-61f04809-9310-4641-95a9-ff5ecebc53cb.png)


일단 이부분에 대해서 url을 맞춰야 할 것 같습니다. (원하시는 url 명으로 하시면 될 것 같아요.)

그리고 입력해서 넘겨주는 채팅의 경우,

Java에서 보통 Json 입력으로 받게할 때는 @RequestBody 라는 것을 사용하는데 이경우는 표기하는 문법이 달라서 RequestBody로 써놓는 것이 맞는지 잘모르겠습니다. 

![실시간 채팅 - 4](https://user-images.githubusercontent.com/97837003/216611220-6408ec5d-a24e-455b-88a1-1ff3001c7315.png)


채팅을 페이지(사람이 보는 화면)에 보여줄 때이고,

![실시간 채팅 - 3](https://user-images.githubusercontent.com/97837003/216611229-5a3c14d7-1d90-40f5-8be9-1829e2ed66b2.png)


다시 서버로 Json 형식으로 보내주는 경우 인데, (실시간으로 주고 받습니다.)

위의 경우 처럼 작동되는 경우 사용자명도 매번 입력하게 되는 것 같은데, 이부분은 조금 아직 고민이 필요합니다.

일단 위의 로직대로라면 입력을 {room_id}로 하고, RequestBody로

```json
{
	"sender" : "유저1",
	"message" : "메세지 입니다."
}
```

처럼 입력을 받고, 이대로 다시 보내주는 것 같습니다.

access token을 사용하는 경우 token에 담긴 유저 정보를 이용해서

“sender” 부분은 제외하고 입력할 수도 있습니다. (토큰을 받아야 서버 이용이 가능한데, 이부분에 유저 정보가 있어서 직접 입력을 하지 않아도 자동으로 입력됩니다.)

kafka 쓰는 경우도 한번 찾아봤는데 시간나시면 아래 글 맨 밑부분 react 봐주세요.
