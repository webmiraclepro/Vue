# vue-beautiful-chat

`vue-beautiful-chat` provides an intercom-like chat window that can be included easily in any project for free. It provides no messaging facilities, only the view component.

`vue-beautiful-chat` is porting to vue of `react-beautiful-chat` (which you can find [here](https://github.com/mattmezza/react-beautiful-chat))

<a href="https://www.npmjs.com/package/vue-beautiful-chat" target="\_parent">
  <img alt="" src="https://img.shields.io/npm/dm/vue-beautiful-chat.svg" />
</a>
<a href="https://github.com/mattmezza/vue-beautiful-chat" target="\_parent">
  <img alt="" src="https://img.shields.io/github/stars/mattmezza/vue-beautiful-chat.svg?style=social&label=Star" />
</a>

Go to [FAQ](#faq) ⬇️

![gif](https://media.giphy.com/media/3ohs4wE4DqXw84xAMo/giphy.gif)

## Features

- Customizeable
- Backend agnostic
- Free

## [Demo](https://mattmezza.github.io/vue-beautiful-chat/)

## Table of Contents
- [Installation](#installation)
- [Example](#example)
- [Components](#api)

## Installation

```
$ yarn add vue-beautiful-chat
```

## Example
```javascript
import Chat from 'vue-beautiful-chat'
Vue.use(Chat)
```

```html
<template>
  <div>
    <beautiful-chat
      :participants="participants"
      :titleImageUrl="titleImageUrl"
      :onMessageWasSent="onMessageWasSent"
      :messageList="messageList"
      :newMessagesCount="newMessagesCount"
      :isOpen="isChatOpen"
      :close="closeChat"
      :open="openChat"
      :showEmoji="true"
      :showFile="true"
      :showTypingIndicator="showTypingIndicator"
      :colors="colors"
      :alwaysScrollToBottom="alwaysScrollToBottom" />
  </div>
</template>
```
```javascript
export default {
  name: 'app',
  data() {
    return {
      participants: [
        {
          id: 'user1',
          name: 'Matteo',
          imageUrl: 'https://avatars3.githubusercontent.com/u/1915989?s=230&v=4'
        },
        {
          id: 'user2',
          name: 'Support',
          imageUrl: 'https://avatars3.githubusercontent.com/u/37018832?s=200&v=4'
        }
      ], // the list of all the participant of the conversation. `name` is the user name, `id` is used to establish the author of a message, `imageUrl` is supposed to be the user avatar.
      titleImageUrl: 'https://a.slack-edge.com/66f9/img/avatars-teams/ava_0001-34.png',
      messageList: [
          { type: 'text', author: `me`, data: { text: `Say yes!` } },
          { type: 'text', author: `user1`, data: { text: `No.` } }
      ], // the list of the messages to show, can be paginated and adjusted dynamically
      newMessagesCount: 0,
      isChatOpen: false, // to determine whether the chat window should be open or closed
      showTypingIndicator: '', // when set to a value matching the participant.id it shows the typing indicator for the specific user
      colors: {
        header: {
          bg: '#4e8cff',
          text: '#ffffff'
        },
        launcher: {
          bg: '#4e8cff'
        },
        messageList: {
          bg: '#ffffff'
        },
        sentMessage: {
          bg: '#4e8cff',
          text: '#ffffff'
        },
        receivedMessage: {
          bg: '#eaeaea',
          text: '#222222'
        },
        userInput: {
          bg: '#f4f7f9',
          text: '#565867'
        }
      }, // specifies the color scheme for the component
      alwaysScrollToBottom: false // when set to true always scrolls the chat to the bottom when new events are in (new message, user starts typing...)
    }
  },
  methods: {
    sendMessage (text) {
      if (text.length > 0) {
        this.newMessagesCount = this.isChatOpen ? this.newMessagesCount : this.newMessagesCount + 1
        this.onMessageWasSent({ author: 'support', type: 'text', data: { text } })
      }
    },
    onMessageWasSent (message) {
      // called when the user sends a message
      this.messageList = [ ...this.messageList, message ]
    },
    openChat () {
      // called when the user clicks on the fab button to open the chat
      this.isChatOpen = true
      this.newMessagesCount = 0
    },
    closeChat () {
      // called when the user clicks on the botton to close the chat
      this.isChatOpen = false
    }
  }
}
```

For more detailed examples see the demo folder.

## Components

# Launcher

`Launcher` is the only component needed to use vue-beautiful-chat. It will react dynamically to changes in messages. All new messages must be added via a change in props as shown in the example.

Launcher props:

|prop | type   | description |
|-----|--------|---------------|
| *participants | [agentProfile] | Represents your product or service's customer service agents. Fields for each agent: id, name, imageUrl|
| *onMessageWasSent | function(message) | Called when a message a message is sent with a message object as an argument. |
| *isOpen | Boolean | The bool indicating whether or not the chat window should be open. |
| *open | Function | The function passed to the component that mutates the above mentioned bool toggle for opening the chat |
| *close | Function | The function passed to the component that mutates the above mentioned bool toggle for closing the chat |
| messageList | [message] | An array of message objects to be rendered as a conversation. |
| showEmoji | Boolean | A bool indicating whether or not to show the emoji button
| showFile | Boolean | A bool indicating whether or not to show the file chooser button
| showTypingIndicator | Boolean | A bool indicating whether or not to show the `typing` indicator
| colors | Object | An object containing the specs of the colors used to paint the component. [See here](#faq)


### Message Objects

Message objects are rendered differently depending on their type. Currently, only text, emoji and file types are supported. Each message object has an `author` field which can have the value 'me' or the id of the corresponding agent.

``` javascript
{
  author: 'support',
  type: 'text',
  data: {
    text: 'some text'
  }
}

{
  author: 'me',
  type: 'emoji',
  data: {
    code: 'someCode'
  }
}

{
  author: 'me',
  type: 'file',
  data: {
    name: 'file.mp3',
    url: 'https:123.rf/file.mp3'
  }
}

```
### FAQ

<details><summary>How to get the demo working?</summary>
<p>

- `cd vue-beautiful-chat`
- `yarn watch` # this starts the compiler so everytime you edit files they get compiled
- `cd demo`
- `yarn dev` # this starts a web server on localhost:8080 so the demo shows up - it also watches for the demo files changes

</p>
</details>

<details><summary>How can I add a feature or fix a bug?</summary>
<p>

- Fork the repository
- Fix/add your changes
- `yarn build` on the root to have the library compiled with your latest changes
- create a pull request describing what you did
- discuss the changes with the maintainer
- boom! your changes are added to the main repo
- a release is created almost once per week 😉

</p>
</details>

<details><summary>How can I customize the colors?</summary>
<p>

- When initializing the component, pass an object specifying the colors used:

```javascript

let redColors = {
  header: {
    bg: '#D32F2F',
    text: '#fff'
  },
  launcher: {
    bg: '#D32F2F'
  },
  messageList: {
    bg: '#fff'
  },
  sentMessage: {
    bg: '#F44336',
    text: '#fff'
  },
  receivedMessage: {
    bg: '#eaeaea',
    text: '#222222'
  },
  userInput: {
    bg: '#fff',
    text: '#212121'
  }
}

<beautiful-chat
      ...
      :colors="redColors" />
```

This is the red variant. Please check [this file](https://github.com/mattmezza/vue-beautiful-chat/tree/master/demo/src/colors.js) for the list of variants shown in the demo page online.

Please note that you need to pass an Object containing each one of the color properties otherwise the validation will fail.

</p>
</details>
