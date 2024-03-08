# vpn
how to creat vpn 

import socket
import threading

# Server configuration
SERVER_HOST = '127.0.0.1'
SERVER_PORT = 12345

# Create a server socket
server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server_socket.bind((SERVER_HOST, SERVER_PORT))
server_socket.listen(5)

print(f"[*] Listening on {SERVER_HOST}:{SERVER_PORT}")

# Client handling thread
def handle_client(client_socket):
    request = client_socket.recv(1024)
    print(f"[*] Received: {request.decode('utf-8')}")

    # Send a simple response
    client_socket.send(b"ACK!")

    client_socket.close()

while True:
    client_sock, addr = server_socket.accept()
    print(f"[*] Accepted connection from: {addr[0]}:{addr[1]}")

    # Start a new thread to handle the client
    client_handler = threading.Thread(target=handle_client, args=(client_sock,))
    client_handler.start()
