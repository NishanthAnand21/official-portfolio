---
layout: page
title: Contact
permalink: /contact/
---

<style>
    #contact-form {
        width: 100%;
        max-width: 600px;
        margin: 0 auto;
    }
    #contact-form label {
        display: block;
        margin-top: 20px;
    }
    #contact-form input[type="text"], #contact-form textarea {
        width: 100%;
        padding: 10px;
        border: 1px solid #ccc;
    }
    #contact-form input[type="submit"] {
        margin-top: 20px;
        padding: 10px 20px;
        background-color: #007BFF;
        color: white;
        border: none;
        cursor: pointer;
    }
    #contact-form input[type="submit"]:hover {
        background-color: #0056b3;
    }
    #success-message {
        display: none;
        margin-top: 20px;
        color: green;
    }
</style>

<h2>Contact Me</h2>

<form id="contact-form" action="https://formsubmit.co/el/xelihi" method="POST">
    <label for="name">Name:</label><br>
    <input type="text" id="name" name="name" placeholder="Enter your name"><br>
    <label for="email">Email:</label><br>
    <input type="text" id="email" name="email" placeholder="Enter your email"><br>
    <label for="message">Message:</label><br>
    <textarea id="message" name="message" placeholder="Enter your message"></textarea><br>
    <input type="submit" value="Submit">
</form>

<div id="success-message">Thank you for your message!</div>

<script>
    document.getElementById('contact-form').addEventListener('submit', function(event) {
        event.preventDefault();

        var formData = new FormData(event.target);
        fetch(event.target.action, {
            method: 'POST',
            body: formData,
            headers: {
                'Accept': 'application/json'
            }
        }).then(response => {
            if (response.ok) {
                document.getElementById('success-message').style.display = 'block';
                event.target.reset();
            } else {
                alert('There was an error submitting your form. Please try again.');
            }
        });

        // Basic validation
        if (!event.target.checkValidity()) {
            alert('Please fill out all fields correctly.');
            return;
        }
    });
</script>
