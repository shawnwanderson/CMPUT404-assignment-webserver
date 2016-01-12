#  coding: utf-8 
# Copyright 2013 Abram Hindle, Eddie Antonio Santos
# 
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
# 
#     http://www.apache.org/licenses/LICENSE-2.0
# 
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
#
# Furthermore it is derived from the Python documentation examples thus
# some of the code is Copyright © 2001-2013 Python Software
# Foundation; All Rights Reserved
#
# http://docs.python.org/2/library/socketserver.html
#
# run: python freetests.py

# try: curl -v -X GET http://127.0.0.1:8080/
#http://docstore.mik.ua/orelly/other/python/0596001886_pythonian-chp-19-sect-2.html
#http://www.opensource.apple.com/source/python/python-3/python/Lib/SimpleHTTPServer.py

import SocketServer
import posixpath

class MyWebServer(SocketServer.BaseRequestHandler):
	protocol_version = "HTTP/1.1"
    
	def handle(self):
		self.data = self.request.recv(1024).strip()
	        print ("Got a request of: \n%s\n" % self.data)
		try:
			(command, path) = self.data.split(" ")[0], self.data.split(" ")[1]
		except:
			self.send_error(404, "File not found")
			return None
		path = self.parsePath(path)
		if command == "GET":
			self.do_GET(path)
		else:
		        self.request.sendall("OK")
	
	def parsePath(self, path):
		basePath = "./www"
		if path == "/":
			path = "/index.html"
		if path[-1] == "/":
			path = path + "index.html"
		path = basePath + path
		path = path.replace("..", "")
		print path
		return path

	def do_GET(self, path):
		try:
			mode = 'rb'
			f = open(path, mode)
		except:
			print("file read error. Should 404")
			self.send_error(404, "File not found")
			return None
		code = 200
		message = "OK"
		content = "\n" + f.read()
		content_type = self.parseType(path)
		self.send_response(code, message, content, content_type)
		return None

	def parseType(self, path):
		ext = path.split(".")[-1]
		if ext == "css":
			return "text/css"
		if ext == "html":
			return "text/html"

	def send_error(self, code, errMessage):
		if code == 404:
			content = "404"
		else:
			content = None
		self.send_response(code, errMessage, content)


	def send_response(self, code, message, content = None, content_type="text/html"):
		if content == None:
			response = "%s %s %s\r\n" %(self.protocol_version, str(code), message)
		else:
			response = "%s %s %s\nContent-Type: %s\n%s\r\n" %(self.protocol_version, str(code), message, content_type, content)
		print "response: \n" + response
		self.request.send(response)
		

if __name__ == "__main__":
    HOST, PORT = "localhost", 8080

    SocketServer.TCPServer.allow_reuse_address = True
    # Create the server, binding to localhost on port 8080
    server = SocketServer.TCPServer((HOST, PORT), MyWebServer)

    # Activate the server; this will keep running until you
    # interrupt the program with Ctrl-C
    server.serve_forever()









