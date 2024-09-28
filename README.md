# THCTT2024-Programming-Hard
Here is code
````py

import socket
import re
import time

morse_dict = {
    ".-": "A", "-...": "B", "-.-.": "C", "-..": "D", ".": "E", "..-.": "F", "--.": "G", "....": "H", 
    "..": "I", ".---": "J", "-.-": "K", ".-..": "L", "--": "M", "-.": "N", "---": "O", ".--.": "P", 
    "--.-": "Q", ".-.": "R", "...": "S", "-": "T", "..-": "U", "...-": "V", ".--": "W", "-..-": "X", 
    "-.--": "Y", "--..": "Z"
}

def rot13_decode(text):
    return text.translate(str.maketrans("ABCDEFGHIJKLMabcdefghijklmNOPQRSTUVWXYZnopqrstuvwxyz", 
                                        "NOPQRSTUVWXYZnopqrstuvwxyzABCDEFGHIJKLMabcdefghijklm"))

def leetspeak_decode(text):
    leet_dict = {'4': 'A', '3': 'E', '1': 'L', '7': 'T', '0': 'O', '5': 'S', '6': 'G', '9': 'P'}
    return ''.join(leet_dict.get(c, c) for c in text)

def morse_decode(morse_code):
    return ''.join(morse_dict.get(c, '') for c in morse_code.split())

def binary_to_ascii(binary_string):
    ascii_chars = [chr(int(b, 2)) for b in binary_string.split()]
    return ''.join(ascii_chars)

def reverse_string(s):
    return s[::-1]

def solve_captcha(captcha_question):
    match_subtraction = re.search(r"What is (\d+) - (\d+)\?", captcha_question)
    if match_subtraction:
        num1, num2 = int(match_subtraction.group(1)), int(match_subtraction.group(2))
        return str(num1 - num2)

    match_multiplication = re.search(r"What is (\d+) \* (\d+)\?", captcha_question)
    if match_multiplication:
        num1, num2 = int(match_multiplication.group(1)), int(match_multiplication.group(2))
        return str(num1 * num2)

    match_addition = re.search(r"What is (\d+) \+ (\d+)\?", captcha_question)
    if match_addition:
        num1, num2 = int(match_addition.group(1)), int(match_addition.group(2))
        return str(num1 + num2)

    return None


def handle_challenge(challenge):
    if "Morse Code" in challenge:
        word = re.search(r"Morse Code\): (.*)", challenge).group(1)
        return morse_decode(word)
    elif "ROT13" in challenge:
        word = re.search(r"ROT13\): (.*)", challenge).group(1)
        return rot13_decode(word)
    elif "Leetspeak" in challenge:
        word = re.search(r"Leetspeak\): (.*)", challenge).group(1)
        return leetspeak_decode(word)
    elif "Binary ASCII" in challenge:
        word = re.search(r"Binary ASCII\): (.*)", challenge).group(1)
        return binary_to_ascii(word)
    elif "Reversed" in challenge:
        word = re.search(r"Reversed\): (.*)", challenge).group(1)
        return reverse_string(word)
    elif "Shuffled" in challenge:
        print("Pass")
        return None
    return None


def test():
    server = "45.76.176.237"
    port = 13339

    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
        s.connect((server, port))
        print("Connected to the server!")

        while True:
            challenge = s.recv(1024).decode()
            print(f"Challenge received: {challenge}")

            if "CAPTCHA" in challenge:
                captcha_response = solve_captcha(challenge)
                if captcha_response:
                    print(f"Solved CAPTCHA: {captcha_response}")
                    s.sendall(captcha_response.encode() + b'\n')
                else:
                    print("Cannot solve CAPTCHA.")
                continue

            response = handle_challenge(challenge)

            if response:
                print(f"Sending response: {response}")
                s.sendall(response.encode() + b'\n')
            else:
                print("Unknoow")

            time.sleep(1)  

test()

```
