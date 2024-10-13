from flask import Flask, render_template, request, redirect, url_for, session, flash
import os
import smtplib
from email.mime.multipart import MIMEMultipart
from email.mime.text import MIMEText
from dotenv import load_dotenv

app = Flask(_name_)
app.secret_key = os.urandom(24)  # For session management

# Load environment variables from .env file
load_dotenv()

# Email credentials from .env file
SMTP_USER = os.getenv("SMTP_USER")
SMTP_PASSWORD = os.getenv("SMTP_PASSWORD")
SMTP_SERVER = 'smtp.gmail.com'
SMTP_PORT = 587

# Dummy login details for one user
USERNAME = "john@email.com"
PASSWORD = "password123"

@app.route('/')
def login():
    return render_template('login.html')

@app.route('/login', methods=['POST'])
def login_post():
    username = request.form.get('email')
    password = request.form.get('password')

    if username == USERNAME and password == PASSWORD:
        session['user'] = username
        return redirect(url_for('dashboard'))
    else:
        flash('Invalid credentials, please try again!')
        return redirect(url_for('login'))

@app.route('/dashboard')
def dashboard():
    if 'user' in session:
        return render_template('dashboard.html')
    else:
        return redirect(url_for('login'))

@app.route('/send_email', methods=['POST'])
def send_email():
    if 'user' in session:
        # Load the email HTML content
        with open('./templates/finalTemplate/email.html', 'r') as file:
            html_content = file.read()

        # Email details
        sender_email = SMTP_USER
        receiver_emails = ['harishkumar.k@necurity.com']  # Update this as necessary
        subject = 'CYBER ALERT'

        # Prepare email message
        msg = MIMEMultipart('alternative')
        msg['Subject'] = subject
        msg['From'] = sender_email
        msg['To'] = ', '.join(receiver_emails)

        html_part = MIMEText(html_content, 'html')
        msg.attach(html_part)

        try:
            with smtplib.SMTP(SMTP_SERVER, SMTP_PORT) as server:
                server.starttls()
                server.login(SMTP_USER, SMTP_PASSWORD)
                server.sendmail(sender_email, receiver_emails, msg

