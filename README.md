# 2b IMPLEMENTATION OF SLIDING WINDOW PROTOCOL
### NAME  : SRISANTH R
### REF NO: 212225240156
## AIM
 Aim of this implementation is to demonstrate the Sliding Window Protocol in computer networks.
## ALGORITHM:
1. Start the program.
2. Get the frame size from the user
3. To create the frame based on the user request.
4. To send frames to server from the client side.
5. If your frames reach the server it will send ACK signal to client
6. Stop the Program
## PROGRAM
### SERVER CODE:
```PY
import socket

def start_server():
    server_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    server_socket.bind(('localhost', 12345))
    print("Server is listening on port 12345")

    expected_frame = 0
    while True:
        data, client_address = server_socket.recvfrom(1024)
        frame_num = int(data.decode())
        print(f"Server received frame: {frame_num}")
        
        # Send ACK for the received frame
        ack = f"ACK {frame_num}"
        server_socket.sendto(ack.encode(), client_address)
        print(f"Server sent {ack}")

if __name__ == "__main__":
    start_server()
```

### CLIENT CODE :
```PY
import socket
import time

def start_client(window_size, total_frames):
    client_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    server_address = ('localhost', 12345)

    base = 0
    next_frame = 0

    while base < total_frames:
        # Send frames within the window
        while next_frame < base + window_size and next_frame < total_frames:
            print(f"Client sending frame: {next_frame}")
            client_socket.sendto(str(next_frame).encode(), server_address)
            next_frame += 1
        
        # Wait for ACKs
        try:
            client_socket.settimeout(2)  # Wait for ACK for 2 seconds
            while True:
                ack, _ = client_socket.recvfrom(1024)
                ack_num = int(ack.decode().split()[1])
                print(f"Client received: {ack.decode()}")
                base = ack_num + 1  # Move the base window up
        except socket.timeout:
            print("Timeout! No ACK received, resending frames.")
            # Resend all frames in the window if no ACK received

        time.sleep(1)  # Wait before sending the next window

    client_socket.close()

if __name__ == "__main__":
    window_size = int(input("Enter the window size: "))
    total_frames = int(input("Enter the total number of frames: "))
    start_client(window_size, total_frames)
```

## OUPUT
### SERVER OUTPUT:

![image](https://github.com/user-attachments/assets/e37eef32-0ee6-4236-89fa-1dbd15f66a04)




### CLIENT OUTPUT:

![image](https://github.com/user-attachments/assets/237ba074-f48b-4ef4-9db7-254acc86c3a1)


## RESULT
Thus, python program to perform stop and wait protocol was successfully executed
