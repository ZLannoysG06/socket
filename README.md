//server_with_threads.py

from socket import *
from fib import fib
from threading import Thread


def fib_server(address):
    sock = socket(AF_INET, SOCK_STREAM)
    sock.setsockopt(SOL_SOCKET, SO_REUSEADDR, 1)
    sock.bind(address)
    sock.listen(5)
    while True:
        client, addr = sock.accept()
        print("Connection", addr)
        Thread(target=fib_handler, args=(client,)).start()


def fib_handler(client):
    while True:
        req = client.recv(100)
        if not req:
            break
        result = fib(int(req))
        resp = str(result).encode("ascii") + b"\n"
        client.send(resp)


fib_server(("", 25000))

//server ตัวนี้ ใช้ โปรแกรมนี้เป็นเซิร์ฟเวอร์ที่รับค่าตัวเลขจากไคลเอนต์ แล้วคำนวณลำดับฟีโบนักชี และส่งผลลัพธ์กลับ ใช้ Thread แยกการเชื่อมต่อแต่ละไคลเอนต์เพื่อให้รองรับหลายคนพร้อมกัน ใช้งานง่าย แค่รัน python server.py และส่งค่าผ่าน TCP พอร์ต 25000 เหมาะสำหรับการทดลองระบบเครือข่ายและการประมวลผลแบบขนาน

