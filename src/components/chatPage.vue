<template>
  <!-- remove_modal -->
  <!-- <div class="background">
    <div class="remove_modal">
      삭제할까요?
      <button>확인</button>
      <button>취소</button>
    </div>
  </div> -->
  <!-- chating -->
  <div style="position:fixed; top:20%; left: 20%;">
    <div class="chat_area_wrap">
      <li class="chat_list" v-for="(chat,i) in chatData" :key="i">
        <div class="user_pofile" :class="{'my_chat': chat.name === this.myName}">
          <div class="img_area" style="margin-right:10px">
            <!-- <img :src="`${chat.img}`" alt=""> -->
            <span>
              {{chat.img}}
            </span>
          </div>
          <div class="nick">
            <span>{{chat.name}}</span>
            <p>
              <span style="background:#fff; display: inline-block; padding: 3px; border-radius: 5px;">
                <div>{{chat.contents}}</div>
              </span>
              <em class="chat_time" style="font-size: 11px;">
                {{chat.time}}
              </em>
              <span class="reply" @click="replyOn(chat, i)">
                답장
              </span>
              <span class="delete" v-if="(chat.remove==false)" @click="removeContent(i)">
                삭제
              </span>
            </p>
          </div>
        </div>
      </li>
    </div>
    
    <!-- text_area -->
    <div class="insert_chat_area">
      <div class="answer" v-if="answerState==true">
        <p style="overflow:hidden">
          <span style="display:block">{{this.repluser}}님에게 답장하기</span>
          <span class="re_con">{{this.repContent}}</span>
        </p>
        <button @click="replyclose">X</button>
      </div>
      <textarea v-model="chating"></textarea>
      <button class="submit_btn" type="submit" @click="chatIn">전송</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'chatPage',
  data() {
    return {
      chatData:[
        {img : 'sample1', name : 'beom', contents : '안녕하세요안녕하세요안녕하세요안녕하세요안녕하세요안녕하세요안녕하세요안녕하세요안녕하세요안녕하세요안녕하세요안녕하세요', time : '14:00',remove:false},
        {img : 'sample2', name : 'sky', contents : '네 안녕하세요', time : '14:00', remove: false},
        {img : 'sample1', name : 'beom', contents : '숙제는', time : '14:03', remove:false},
        {img : 'sample1', name : 'beom', contents : '다해 오셨나요?', time : '14:10', remove:false},
        {img : 'sample2', name : 'sky', contents : '네!', time : '14:10', remove:false},
        {img : 'sample2', name : 'sky', contents : '다했습니다', time : '14:10', remove:false},
        {img : 'sample2', name : 'sky', contents : '더 할거 없나요?', time : '14:11', remove:false},
        {img : 'sample1', name : 'beom', contents : '아니요 충분합니다', time : '14:14', remove:false},
      ],
      chating: '',
      answerState: false,
      repluser: '',
      repContent:'',
      myName: 'sky',
    }
  },

  methods: {
    timer(dataIdx) {
      setTimeout(()=>{
        let idx = dataIdx[0]-1
        console.log(dataIdx[0])
        console.log('dataIdx',dataIdx)
        console.log(idx)
        console.log(parseInt(idx))
        console.log(Number(idx))
        console.log('5초지남')
        console.log(this.chatData)
        this.chatData[idx].remove = true
      },5000)
    },

    chatIn() {
      let newChat = {
        img: 'sample2',
        name: 'sky',
        contents: this.chating,
        time: '14:15',
        remove: false
      }
      if(this.chating=='') {
        return false;
      }
      this.chatData.push(newChat)
      this.chating = '';
      let dataIdx = [this.chatData.length]
      console.log(dataIdx)
      this.answerState = false;
      this.timer(dataIdx);
      console.log(this.timer)
    },
    replyOn(chat, i) {
      this.repluser = this.chatData[i].name
      this.repContent = this.chatData[i].contents
      this.answerState = true
    },
    replyclose() {
      this.answerState = false
    },
    removeContent(i) {
      this.chatData[i].contents = '삭제된 메세지입니다.'
      setTimeout(()=> {
        this.chatData.splice(i,1)
      },5000)
    }
  }

}
</script>

<style>
  * {
    margin: 0;
    padding: 0;
  }
  ul, ol, li{
    list-style: none;
  }
  .chat_area_wrap {
    position: relative;
    overflow-y: scroll;
    height: 400px;
    width: 390px;
    border: 1px solid #000;
    background: rgb(174, 234, 250);
  }
  .chat_list {
    margin-bottom: 10px;
  }
  .img_area img{
    width: 60px;
    border-radius: 50%;
  }
  .user_pofile {
    display: flex;
    justify-content: start;
  }
  .insert_chat_area textarea {
    border: 1px solid #000;
    width: 390px;
    height: 70px;
  }
  .answer {
    display: flex;
    width: 99%;
    height: 40PX;
    align-items: center;
    justify-content: space-between;
    position:absolute;
    bottom: 85px;
    background: rgba(255, 255, 255, 0.8)
  }
  .answer button {
    margin-right: 20px;
    background: none;
    outline: none;
    border: none;
  }
  .chat_list:hover .reply,
  .chat_list:hover .delete {
    display: inline;
  }
  .reply,
  .delete {
    display: none;
  }
  .re_con {
    display: block;
    text-overflow: ellipsis;
    overflow: hidden;
    white-space: nowrap;
  }
  .my_chat {
    justify-content: end;
  }
  .my_chat .nick > span{
    display: none;
  }
  .my_chat .img_area {
    display: none;
  }
  .my_chat .nick p {
    display: flex;
    align-items: center;
  }
  .my_chat .nick p span {
    display: none;
  }
  .my_chat .nick p em {
    align-self: flex-end;
  }
  .my_chat .nick p span:nth-child(3) {
    order: -1;
    margin-left: 10px;
  }
  .my_chat .nick p span:nth-child(4) {
    order: -2;
  }
</style>