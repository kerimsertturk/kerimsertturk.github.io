---
layout: "page"
title: "Contact"
permalink : /contact/
---

Send me an email and I will get back to you as soon as possible!

<html>

<form class="contactme" action="https://formspree.io/xdqvqpym" method="POST">
    <input type="text" name="email" placeholder="Email Address">
    <textarea type="text" name="content" rows="7" placeholder="Message"></textarea>
    <input type="hidden" name="_next" value="<REDIRECTION LINK> ">
    <input type="hidden" name="_subject" value="New Form Submission">
    <input type="text" name="_gotcha" style="display:none">
    <input type="submit" value="Submit">
</form>

<style>
form.contactme [name="content"] {
    font-family: Arial;
}
form.contactme input[type="text"], form.contactme textarea[type="text"] {
    width: 100%;
    vertical-align: middle;
    margin-top: 10px;
    margin-bottom: 10px;
    padding: 0.75em;
    /*font-family: monospace, sans-serif;
    font-weight: lighter;*/
    border-style: solid;
    border-color: black;
    outline-color: black;
    border-width: 1px;
    border-radius: 3px;
    transition: box-shadow .2s ease;
}
form.contactme input[type="submit"] {
    outline: none;
    color: white;
    background-color: gray;
    border-radius: 3px;
    padding: 1em;
    margin: 10px 0 0 0;
    border: 1px solid transparent;
    height: auto;
}
</style>

</html>
