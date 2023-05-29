# Amir

        Add a user to the conversation, or update their user/admin status.

        Args:
            id (str): user identifier to invite
            admin (bool): whether the user will gain admin privileges
        """
        self.skype.conn("PUT", "{0}/threads/{1}/members/8:{2}".format(self.skype.conn.msgsHost, self.id, id),
                        auth=SkypeConnection.Auth.RegToken, json={"role": "Admin" if admin else "User"})
        if id not in self.userIds:
            self.userIds.append(id)
        if admin and id not in self.adminIds:
            self.adminIds.append(id)
        elif not admin and id in self.adminIds:
            self.adminIds.remove(id) 
@gamli_is_in_love_with_Ler
addChatroomMember(self,chatRoomName,userNames):
        url = self.base_uri + '/webwxupdatechatroom?fun=addmember&pass_ticket=%s' % (self.pass_ticket)
        params = {
            'BaseRequest': self.BaseRequest,
            'ChatRoomName': chatRoomName,
            'AddMemberList': ','.join(userNames),
        }

        dic = self._post(url,params)

        state = True if dic['BaseResponse']['Ret'] == 0 else False
        errMsg = dic['BaseResponse']['ErrMsg']
        memberList = dic['MemberList']
        deletedList = []
        blockedList = []
        for member in memberList:
            if member['MemberStatus'] == 4: #被对方删除了
                deletedList.append(member['UserName'])
            elif member['MemberStatus'] == 3: #被加入黑名单
                blockedList.append(member['UserName'])


        return state,errMsg,deletedList,blockedList 
