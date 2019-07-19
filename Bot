from flask import Flask, flash, render_template, request, jsonify, redirect, url_for, make_response
from flask_socketio import SocketIO, send, emit, ConnectionRefusedError
import atexit
import os
import requests
import json
import ast
[6/27 11:07 AM] Jonathan Booth
    

<!DOCTYPE html>
<html><head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>index</title>
<!-- Global site tag (gtag.js) - Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id=UA-91536704-3"></script>
    <script>
window.dataLayer = window.dataLayer || [];
function gtag(){dataLayer.push(arguments);}
      gtag('js', new Date());
gtag('config', 'UA-91536704-3');
</script>
<!-- websocket -->
<script src="//cdnjs.cloudflare.com/ajax/libs/socket.io/2.2.0/socket.io.js" integrity="sha256-yr4fRk/GU1ehYJPAs8P4JlTgu0Hdsp4ZKrx8bDEDC3I=" crossorigin="anonymous"></script>
<!-- Bootstrap -->
<link href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
<!--Site Style-->
<link href="{{ url_for('static', filename = 'styles.css') }}" rel="stylesheet">
<!-- jQuery (necessary for Bootstrap's JavaScript plugins) -->
<script src="https://ajax.googleapis.com/ajax/libs/jquery/1.12.4/jquery.min.js"></script>
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.2.1/jquery.min.js"></script>
<style>
@import url('https://fonts.googleapis.com/css?family=Kalam');
::-webkit-scrollbar {
        width: 0px;  /* remove scrollbar space */
background: transparent;  /* optional: just make scrollbar invisible */
}
    /* optional: show position indicator in red */
::-webkit-scrollbar-thumb {
        background: #FF0000;
}
    body{
        background: #fff;
background: url("http://cidmedia.clayton.edu/IMG/bg-grey.jpg") no-repeat fixed center;
background-size: cover;
}
    #bottom{
        margin-top: 5vh;
}
    #buttonInput {
        background:#5cb85c;
border:0 none;
cursor:pointer;
-webkit-border-radius: 5px;
border-radius: 5px;
color: #fff;
box-shadow: 5px 5px #888888;
}
    footer{
        padding-top: 5px;
}
    .glyphicon-refresh{
        font-size: 20px;
font-weight: 900;
line-height: 0;
color: #222222;
text-shadow: 1px 2px #888888;
}
    .sticky {
      position: fixed;
top: 0;
width: 100%;
}
</style>
</head>
<body>
    <div align="right" class="sticky" id="">
        <a href="{{url_for('index')}}" target="_self"><span class="glyphicon glyphicon-refresh" style="padding:20px;"></span></a>
    </div>
    <div class="chat-box">
        <div id="chatbox">
            <p class="botText"> {{botText}} </p>
        </div>
        <footer id="userInput" class="footer">
            <input id="textInput" type="text" name="msg" placeholder="Message" style="font-family: 'Kalam', cursive;">
            <button id="buttonInput" type="submit"><span class="hidden-xs">Send </span><span class="glyphicon glyphicon-send" ></span></button>
        </footer>
        <div id="bottom"></div>
    </div>
    <script type="text/javascript" charset="utf-8">
        var liveAgent = false;
var socket = io();
var cval = "";
socket.on('connect', function() {
            ibm_cval = getCookie('IBM-sessionId');
socket.emit('json', {ibm: ibm_cval + " connected"});
});
socket.on('message', function(data){
            socket.emit('json', data + "okay!");
//update page content
if(data != undefined){
            var agentHtml = '<div class="botText">'+data+'</div>';
$("#chatbox").append(agentHtml);}
            liveAgent = true;
scrollDown();
});
socket.on('disconnect', function() {
            socket.emit('json', {ibm: ibm_cval + ' disconnected'});
});
$(document).ready(function() {
            //document is loaded and DOM is ready
setInterval(botTextChecker,100);
});
function botTextChecker(){
            var x = $("#chatbox .botText:last-child").html()
            if(x == undefined){
                scrollDown();
}
        };
function scrollDown(){
            $('html,body').animate({
                scrollTop: $("#bottom").offset().top
});
}
        function getBotResponse() {
          var rawText = $("#textInput").val();
var userHtml = '<div class="userText"><span style="font-family: \'Kalam\', cursive;">' + rawText + '</span></div>';
$("#textInput").val("");
$("#chatbox").append(userHtml);
if(liveAgent != true){
              $.get("/get",{ msg: rawText }).done(function(data) {
                var botHtml = '<div class="botText">'+data+'</div>';
$("#chatbox").append(botHtml);
});
}
        }
        $("#textInput").keypress(function(e) {
            if(e.which == 13) {
                getBotResponse();
scrollDown();
}
        });
$("#buttonInput").click(function() {
          getBotResponse();
scrollDown();
})
       function getCookie(cookie) {
        return document.cookie.split(';').reduce(function(prev, c) {
        var arr = c.split('=');
return (arr[0].trim() === cookie) ? arr[1] : prev;
}, undefined);
}
    </script></body>
</html>


​[6/27 11:08 AM] Jonathan Booth
    

html {
  position: relative;
min-height: 100%;
}
body {
  margin-bottom: 60px;
font-family: Helvetica;
height: 100%;
width:100%;
position: center;
text-align:center;
}
.header {
  position: -webkit-sticky;
position: sticky;
top: 0;
width: 100%;
height: 70%;
line-height: 50%;
text-align: center;
background-color: #ddd;
}
.footer {
  position: fixed;
bottom: 0;
width: 100%;
height: 60px;
line-height: 4rem;
text-align: center;
background-color: #ddd;
}
.helloInput {
  width: 300px;
}
.container{
  text-align: center;
}
h1 {
    color: black;
margin-bottom: 0;
margin-top: 0;
text-align: center;
font-size: 40px;
}
h3 {
    color: black;
font-size: 20px;
margin-top: 3px;
text-align: center;
}
#chatbox {
    margin-left: 5%;
margin-right: 5%;
width: 90%;
margin-top: 80px;
}
#userInput {
    margin-left: auto;
margin-right: auto;
width: 100%;
margin-top: 60px;
background: rgba(192,192,192,0.7);
}
#textInput {
    width: 80%;
border: none;
font-family: monospace;
font-size: 2rem;
box-shadow: 5px 5px #888888;
padding-left: 10px;
}
#buttonInput {
    padding: 3px;
font-family: monospace;
font-size: 17px;
width: 15%
}
.userText {
    color: white;
text-shadow: 2px 1px #000;
font-family: monospace;
font-size: 17px;
text-align: right;
line-height: 30px;
background-color: #092c74;
margin: 0px 0px 10px 15%;
min-height: 80px;
border-radius: 10px 10px 0px 10px;
padding: 15px;
box-shadow: 3px 2px #aaa;
}
.userText span {
    padding: 10px;
border-radius: 2px;
}
.botText {
    color: #fff;
text-shadow: 2px 1px #092c74;
font-family: monospace;
font-size: 17px;
text-align: left;
line-height: 30px;
background-color: #fc6719;
margin: 0px 15% 10px 0px;
min-height: 80px;
border-radius: 10px 10px 10px 0px;
padding: 15px;
box-shadow: 3px 3px #AAA;
}
#wrapper {
      min-width: 250px;
margin-top: -4%;
margin-bottom:-7%;
}
.botText span {
    padding: 10px;
border-radius: 2px;
}


