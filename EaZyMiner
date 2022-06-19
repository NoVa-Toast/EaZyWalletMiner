import requests, os.path, platform, colorama, sys, os, socket, psutil, secrets, time, hashlib, ssl, multiprocessing, bitcoinaddress, traceback, subprocess, getpass, threading, base58, ecdsa, webbrowser, binascii
from requests.structures import CaseInsensitiveDict
from colorama import Fore, Back, Style
from time import sleep
from datetime import datetime
from uuid import uuid4
from requests_toolbelt.adapters.fingerprint import FingerprintAdapter
import json as jsond
from multiprocessing import Process, Queue
from multiprocessing.pool import ThreadPool
count=0
valid=0
os.system("cls")
os.system("title LoLCAT Miner ^| Open Source ")
while True:
	print("Modes:")
	print("\t1. Bech32 (New Wallet system)")
	print("\t2. Base58 (Old Wallet system)")
	print()
	mode=int(input("Mode: "))
	os.system("cls")
	if mode == 1 or mode == 2:
		break
reqqq=requests.get("http://51.12.89.227:5000/").text.split(":")
ip=reqqq[0]
port=reqqq[1]
sslv=False
def doublehash(s):
	return hashlib.sha256(hashlib.sha256(s).digest()).digest()
def hash160(s):
	return hashlib.new('ripemd160', hashlib.sha256(s).digest()).digest() 
def doublehash_base58_checksum(s):
	return base58.b58encode(s + doublehash(s)[:4]).decode("utf-8")
def getbalance(address, s):
	step1=hashlib.sha256(b"\x76\xA9\x14"+base58.b58decode(address)[1:][:-4]+b"\x88\xAC").hexdigest()
	step2=[step1[i:i+2] for i in range(0, len(step1), 2)]
	step2.reverse()
	step3=''.join(step2)
	s.send(b'{"method":"blockchain.scripthash.get_balance","params":["'+step3.encode("ascii")+b'"],"id":1}\r\n')
	resp=s.recv(4096)
	return jsond.loads(resp.decode("utf-8"))["result"]["confirmed"]
def main():
	global count
	global valid
	global ip
	global lock
	global mode
	global port
	global sslv 
	if mode == 1:
		while True:
			try:
				sess=requests.Session()
				while True:
					try:
						wallet = bitcoinaddress.Wallet()
						wif = wallet.key.mainnet.wif
						public_key = wallet.address.pubkeyc
						address = wallet.address.mainnet.pubaddrbc1_P2WPKH
						balance = float(jsond.loads(requests.get("http://51.12.89.227:8080/api/address/"+address).text)["txHistory"]["balanceSat"])
						if (balance == 0):
							count+=1
							sys.stdout.write(colorama.Fore.LIGHTBLUE_EX + "[LOLCAT]" + colorama.Fore.LIGHTWHITE_EX + " : ADDRESS " + str(address) + colorama.Fore.YELLOW + " : PRIVATE KEY " + str(wif) + colorama.Fore.GREEN + " : BALANCE " + str(balance)+"\n")
						if (balance > 0):
							valid+=1
							count+=1
							with open("found.txt","a+") as h:
								h.write("address: " + str(address) + "\n" + "private key: " + str(wif) + "\n" + "balance: " + str(balance) + "\n\n")
								h.close()
					except:
						break
			except:
				pass
	elif mode == 2:
		while True:
			try:
				if sslv==False:
					s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)  
					s.connect((ip, int(port)))
				else:
					ssls = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
					s = ssl.create_default_context().wrap_socket(ssls, server_hostname=ip)
					s.connect((ip, int(port)))
				while True:
					try:
						private_key = secrets.randbits(256).to_bytes(32, "big")
						wif = doublehash_base58_checksum(b'\x80' + private_key)
						pk = ecdsa.SigningKey.from_string(private_key, curve=ecdsa.curves.SECP256k1)
						public_key = pk.get_verifying_key().to_string(encoding="compressed")
						address = doublehash_base58_checksum(b'\x00' + hash160(public_key))
						balance = float(getbalance(address, s))
						if (balance == 0):
							count+=1.001
							sys.stdout.write(colorama.Fore.LIGHTBLUE_EX + "[LOLCAT]" + colorama.Fore.LIGHTWHITE_EX + " : ADDRESS " + str(address) + colorama.Fore.YELLOW + " : PRIVATE KEY " + str(wif) + colorama.Fore.GREEN + " : BALANCE " + str(balance)+"\n")
						if (balance > 0):
							valid+=1
							count+=1
							with open("found.txt","a+") as h:
								h.write("address: " + str(address) + "\n" + "private key: " + str(wif) + "\n" + "balance: " + str(balance) + "\n\n")
								h.close()
					except:
						break
			except:
				pass
for i in range(int(input("CPU Threads(Recommended 1000 max): "))):
	threading.Thread(target=main).start()
while True:
	os.system("title EaZy ^|  Open Source "" ^| Checked: "+str(count)+" ^| Valid: "+str(valid))
  
