import socket
import sys
import re
import time
from thread import *
#///////////////////////////////

# In here, we have our clients class, which we can save their attributes easily

class clients:
     connection = 0
     address = ""
     nickname = ""
     username = ""
     realname = ""
     hostname = ""
     ip = ""
     currentchannel = ""
     def __init__(self,c,a,n,u,h,r,i):#constructor for which we use in the initial state when user connects to the server!
         self.connection = c
         self.address = a
         self.nickname = n
         self.username = u
         self.realname = r
         self.hostname = h
         self.ip = i
     def clientInfo(self):#returns an array with the full info of the client!

        fullInfo = []
        fullInfo.append(self.connection)
        fullInfo.append(self.nickname)
        fullInfo.append(self.username)
        fullInfo.append(self.address)
        return fullInfo
     def clientNickname(self):
         return self.nickname
class channels:
    name = ""
    users = []
    host = 0
    def __init__(self,n,h):
     self.name = n
     self.host = h
     self.users = []


#/////////////////////////

#we define the host and port for our server to listen to!
HOST = ''
PORT = 6667
serverName = "H.net"
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
print 'Socket created'

try:
    s.bind((HOST, PORT))
except socket.error , msg:#catch bind exception such as already in use socket for which we are using!
    print 'Bind failed. Error Code : ' + str(msg[0]) + ' Message ' + msg[1]
    sys.exit()

print 'Socket bind complete'

s.listen(10)
print 'Socket now listening'
#dnsServerIp = raw_input("Dns Server Ip : ")
#Send Address of the server to the DNS server

#Client_socket = socket.socket(socket.AF_INET,socket.SOCK_STREAM)
#try:
 #   Client_socket.connect((dnsServerIp,5656))
#except socket.error, msg:
 #     print msg

#temp = "Server " +serverName + " " + str(PORT)
#Client_socket.send(temp)
#data = Client_socket.recv(2000)
#print data

temp  = []
temp2 = []
myclients = []
closeflag = 0

ct = [] #array which will contain client objects that are created when they are connected to the server!
channel = []#array which will contain channel objects
#my client function which recieves the connection after it is accepted and recieves the incoming data from the clinets that
# are connected to the server!
def client(connection,address):
    flagregister = 0
    nickflag = 0
    nicknamegenerator = 0
    newchannel = 1
    inchannel = 0
    nicknames= ""
    while 1:

        data = connection.recv(1024)
        if(re.search("WHO",data)):
                temp = data.split(" ")[1].split("#")[1].split("\r")[0]
                channelname = temp

                for x in channel:
                    if x.name == temp:
                        for z in x.users:
                                                if z.nickname != x.host:
                                                    temp = " " + z.nickname
                                                    nicknames += temp
                        temp = ":H.net 353 " + mynickname + " = #" + x.name + " :"+ nicknames[1:] + " @" + x.host + "\n\r"
                        myobject.connection.send(temp)
                        temp = ":H.net 366 " + mynickname + " #" + x.name + " :End of /NAMES list." + "\n\r"
                        myobject.connection.send(temp)
                        temp = ":H.net 329 " + mynickname + " #" + x.name + " 1433771297" + "\n\r"
                        myobject.connection.send(temp)
                        for y in x.users:
                               if y.nickname == x.host:
                                    temp = ":H.net 352 " + mynickname + " #" + x.name + " ~" + y.username + " " + y.ip + " H.net " + x.host + " H @:2 " + y.realname + "\n\r"
                                    myobject.connection.send(temp)
                               else:
                                temp = ":H.net 352 " + mynickname + " #" + x.name + " ~" + y.username + " " + y.ip + " H.net " + y.nickname + " H :0 " + y.realname + "\n\r"
                                myobject.connection.send(temp)
                temp = ":H.net 315 "+ mynickname + " #" + channelname + " :End of /WHO list." + "\n\r"
                myobject.connection.send(temp)
                nicknames=""


        if(re.search("PART",data)):
            for x in channel:
                    if x.name == myobject.currentchannel:
                        for y in x.users:
                            if y.nickname == mynickname:
                                if(mynickname == x.host):
                                    x.users.remove(y)
                                    if  not x.users:
                                        channel.remove(x)
                                    else:
                                        y.hostname = x.users[0]
                                else:
                                    x.users.remove(y)
                                    myobject.currentchannel = ""
                            else:
                                temp = ":" + mynickname + "!~" + myobject.username+"@"+myobject.ip +" " + data
                                y.connection.send(temp)


        if(re.search("LIST",data)):

            for x in channel:
                    MYtemp ="Channel " + str(x.name) + " UserCount: " + str(len(x.users)) + "\n\r\n\r"
                    connection.send(MYtemp)



        #Join command which, if the name of the channel doesn't exist it creates a channel
        if(re.search("JOIN",data)):
          if(data[5] != "#"):
              mydata = "Please use # sign before channel Name!\n\r"
              connection.send(mydata)
          else:
            temp = data.split()[1].split("#")[1]

            if(inchannel==0):
                  for x in channel:
                        if x.name == temp:
                                newchannel = 0
                                x.users.append(myobject)
                                myobject.currentchannel = x.name

                                for y in x.users:

                                            #temp = mynickname + " Has Joined The Channel " + x.name + "\n\r"
                                            temp = ":" + mynickname+"!~"+myobject.username+"@"+myobject.ip+ " JOIN :"+"#"+ x.name + "\n\r"
                                            y.connection.send(temp)
                                            for z in x.users:
                                                if z.nickname != x.host:
                                                    temp = " " + z.nickname
                                                    nicknames += temp
                                            #print nicknames
                                            temp = ":H.net 353 " + mynickname + " = #" + x.name + " :"+ nicknames[1:] + " @" + x.host + "\n\r"
                                            y.connection.send(temp)
                                            temp = ":H.net 366 " + mynickname + " #" + x.name + " :End of /NAMES list." + "\n\r"
                                            y.connection.send(temp)
                                            temp = ":H.net 324 " + mynickname + " #" + x.name + " +nt" + "\n\r"
                                            y.connection.send(temp)
                                            nicknames=""
                                inchannel=1

                  if(newchannel == 1):
                    newchannel = channels(temp,mynickname)
                    channel.append(newchannel)
                    #temp = "You have created channel : " + newchannel.name + "\n\r"
                    temp = ":" + mynickname+"!~"+myobject.username+"@"+myobject.ip+ " JOIN :"+"#"+newchannel.name + "\n\r"
                    connection.send(temp)
                    temp = ":H.net 366 " + mynickname + " #" + newchannel.name + " :End of /NAMES list." + "\n\r"
                    connection.send(temp)
                    temp = ":H.net" + " MODE #" + newchannel.name + " +nt" + "\n\r"
                    connection.send(temp)
                    newchannel.users.append(myobject)
                    myobject.currentchannel = newchannel.name
                    inchannel=1

            else:
                for x in channel:
                    if x.name == myobject.currentchannel:
                        for y in x.users:
                            if y.nickname == mynickname:
                                if(mynickname == x.host):
                                    x.users.remove(y)
                                    if  not x.users:
                                        channel.remove(x)
                                    else:
                                        y.hostname = x.users[0]
                                else:
                                    x.users.remove(y)
                                    myobject.currentchannel = ""
                            else:
                                temp = ":" + mynickname + "!~" + myobject.username+"@"+myobject.ip +" " + data
                                y.connection.send(temp)
                for x in channel:
                  for x in channel:
                        if x.name == temp:
                                newchannel = 0
                                x.users.append(myobject)
                                myobject.currentchannel = x.name

                                for y in x.users:

                                            #temp = mynickname + " Has Joined The Channel " + x.name + "\n\r"
                                            temp = ":" + mynickname+"!~"+myobject.username+"@"+myobject.ip+ " JOIN :"+"#"+ x.name + "\n\r"
                                            y.connection.send(temp)
                                            for z in x.users:
                                                if z.nickname != x.host:
                                                    temp = " " + z.nickname
                                                    nicknames += temp
                                            #print nicknames
                                            temp = ":H.net 353 " + mynickname + " = #" + x.name + " :"+ nicknames[1:] + " @" + x.host + "\n\r"
                                            y.connection.send(temp)
                                            temp = ":H.net 366 " + mynickname + " #" + x.name + " :End of /NAMES list." + "\n\r"
                                            y.connection.send(temp)
                                            temp = ":H.net 324 " + mynickname + " #" + x.name + " +nt" + "\n\r"
                                            y.connection.send(temp)
                                            nicknames=""
                                inchannel=1

                  if(newchannel == 1):
                    newchannel = channels(temp,mynickname)
                    channel.append(newchannel)
                    #temp = "You have created channel : " + newchannel.name + "\n\r"
                    temp = ":" + mynickname+"!~"+myobject.username+"@"+myobject.ip+ " JOIN :"+"#"+newchannel.name + "\n\r"
                    connection.send(temp)
                    temp = ":H.net 366 " + mynickname + " #" + newchannel.name + " :End of /NAMES list." + "\n\r"
                    connection.send(temp)
                    temp = ":H.net" + " MODE #" + newchannel.name + " +nt" + "\n\r"
                    connection.send(temp)
                    newchannel.users.append(myobject)
                    myobject.currentchannel = newchannel.name
                    inchannel=1
        #if user sends the nick command we should change the nickname we search for his old nickname and change it!
        #the flag is explained in the next if statement!
        if(re.search("NICK",data) and flagregister==1):
            temp = data.split()
            nickflag=0
            for x in ct:
                if x.nickname == temp[1]:
                    nickflag = 1
                    connection.send("Nickname is already in use by another user\n\r")
            if nickflag==0:
             for x in ct:
                if x.nickname == mynickname:
                    x.nickname = temp[1]
                    temp = ":" + mynickname+"!~"+myobject.username+"@"+myobject.ip+ " NICK :" + x.nickname + "\n\r"
                    x.connection.send(temp)
                    temp = "Your Nickname Has Changed To: " + x.nickname + "\n\r"
                    mynickname = x.nickname
                    x.connection.send(temp)
                    nickflag=0


       #User can set his/her Username,Hostname,Real name
        if(re.search("USER",data) and flagregister==1 ):
                temp = data.split(' ')
                for x in ct:
                    if x.nickname == mynickname:
                        x.username = temp[1]
                        x.hostname = temp[2]
                        x.realname = temp[3]
                        temp = "Your information has been changed!\n\r"
                        x.connection.send(temp)

        #if the incoming data contains "NICK" then it means it's the first time that he/she is connected and we should save
        #the info! that is the nickname, connection,username, etc
        #the flag is for knowing that the user is connected for the first time!

        if(re.search("NICK",data) and flagregister==0 ):
            temp = data.split(' ')
            temp2 = temp[1].split('\r')
            mynickname = temp2[0]
            realname = data.split(':')[1]
            #print temp

            for x in ct:
                if x.nickname == mynickname:#if a users nickname is already in our data base, then we set a nickname for him/her and
                    #tell them to change it if they want!
                    connection.send("Nickname is already used, please change your nickname with '/NICK <nickname>'. ")
                    nickflag = 1
            if nickflag==1:
                x = clients(connection,address,"Nickname"+str(nicknamegenerator),temp[2],temp[3],realname,temp[4])
                mynickname = x.nickname
                myclients.append(x)
                nicknamegenerator =nicknamegenerator+1
                nickflag=0
                myobject = x

            else:
                x = clients(connection,address,temp2[0],temp[2],temp[3],realname,temp[4])
                myclients.append(x)
                myobject = x
            ct.append(x)
            flagregister=1
            connection.send("Welcome " + mynickname + "\n\r")
        #Show's the user Information
        if(re.search("SHOWME",data)):

                for x in ct:
                   if x.nickname == mynickname:
                       temp = "Nickname: " + str(x.nickname) + " Username: " + str(x.username) + " Hostname: " + str(x.hostname) + " Realname: " + str(x.realname) + "Current Channel:  " + str(x.currentchannel) + "\n\r"
                       x.connection.send(temp)
        #if the data string contains show we send the already connected client's s nickname to the client that requested!
        if(re.search("shownick", data)):
                mystring = ""
                counter = 1
                for x in ct:

                    mystring = str(counter) + "." + x.nickname + " " + mystring
                    counter = counter +1
                mystring = mystring + "\n\r"
                connection.send(mystring)


        if(re.search("Send",data)):

            temp = data.split(' ')
            temp2 = data.split(':')
            foundUserFlag =0

            for x in myclients:
                if(x.nickname == temp[4].split(':')[0].split('\n')[0]):
                    tmp = "1"
                    connection.send(tmp)
                    print "got here"
                    t = temp[2] + ":" + temp2[1] + "\n\r"
                    x.connection.send(t)
                    foundUserFlag = 1
            if(foundUserFlag == 0):
                t = '0'
                connection.send(t)

        #if the data contains PRIVMSG then it seeks the nickname that has been sent to it. and if found sends the data if not
        #returns a failure message
        if(re.search("PRIVMSG",data)):
            foundflag = 0
            if(data[8] == "#"):
                  channeltosendto = data.split(' ')[1].split('#')[1]
                  msgtosendto = data.split(':')[1:]
                  for x in channel:
                      if x.name == channeltosendto:
                          for y in x.users:
                              if(y.nickname != mynickname):
                                   temp = ":"+mynickname + "!~"+myobject.username+"@"+myobject.ip+ " " +data
                                   y.connection.send(temp)

            else:

                temp = data.split(' ')
                flag = 0
                t = temp[1].split(':')
                nicknametosend = t[0]
                #print "Nickname to send to: " + nicknametosend
                temp2 = data.split(':')
                print temp2
                for x in ct:
                    if str(nicknametosend) == str(x.nickname):
                        t = mynickname + ":" + temp2[1] + "\n\r"
                        x.connection.send(t)
                        flag=1

                if(flag==0):
  #                  userexist = 0
   #                 temp = "Get"
    #

  #                      tmp = Client_socket.recv(1024)

#                        if(tmp=='$'):
 #                           userexist=0
  #                          break
   #                     if(len(tmp) > 1 & (tmp[-1]== '$')):
    #                            tmp2 = tmp.split('$')[0].split('|')
     #                   else: tmp2 = tmp.split('|')
      #                  tmpAddr = tmp2[0]
       #                 tmpPort = tmp2[1]

        #                mysocket = socket.socket(socket.AF_INET,socket.SOCK_STREAM)
         #               if(tmpAddr == '127.0.0.1'):
          #                  tmpAddr = dnsServerIp
           #             try:
            #                mysocket.connect((tmpAddr,int(tmpPort)))
             #           except socket.error, msg:
              #              print msg
               #         tmp = "Send from " + mynickname + " to " + nicknametosend + ":" + temp2[1]
                #        mysocket.send(tmp)
                 #       tmp = mysocket.recv(1024)
                  #      if(re.search("1",tmp)):
                   #         userexist = 1
                    #        tmp = "Close"
                     #       mysocket.send(tmp)
                      #      mysocket.close()
                       #     break

   #                 if(userexist == 0):
                        temp3 = "There is no user with that nickname + \n\r"
    #                    mysocket.close()
                        connection.send(temp3)

        flag=0

        #if the data contains BROADCAST, then it searchs the clients that are connected and sends the message to all of them!
        if(re.search("BROADCAST",data)):
                temp = data[10:]
                print "Data to broadcast: " + temp
                temp = mynickname + " : " + temp + "\n\r"
                for x in ct:
                    x.connection.send(temp)
        #if a client quits, then it's object is deleted!
        if(re.search("QUIT",data)):
            for x in ct:
                if x.nickname == mynickname:
                    ct.remove(x)
                    del x
                    #print "quit!"
                    for y in ct:
                      temp = mynickname + " has been disconnected!" + "\n\r"
                      y.connection.send(temp)
                    break


            connection.close()
            break
        if(re.search("Close",data)):
            closeflag = connection

            break;
        print "Incoming data: " + data
if(closeflag != 0):
    closeflag.close()
    print "Closed"
    closeflag = 0
while 1:
    #server accepts the client and runs a thread on the function above!
    conn, address = s.accept()
    start_new_thread(client,(conn,address,))

s.close()