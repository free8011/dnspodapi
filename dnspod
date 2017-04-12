
import requests
import json
from pprint import pprint
requests.packages.urllib3.disable_warnings()

class Dnspodapi:
    def __init__(self,login_email='',login_password='',domain='',serverip='',record_type='A',ttl='60',record_line='default'):
        self.url = 'https://api.dnspod.com/'

        self.domain = domain
        self.serverip = serverip
        self.record_type = record_type
        self.ttl = ttl
        self.record_line = record_line
        self.usertoken = self.auth(login_email,login_password)
        self.domainid = self.mydomain()

    def auth(self,login_email,login_password):
        url = '%sAuth'% self.url
        data = {'login_email': login_email, 'login_password': login_password, 'format': 'json'}
        rtdata = self.responced(url, data)
        return rtdata['user_token']

    def version(self):
        url = '%sinfo.Version'% self.url
        data = {'user_token': self.usertoken, 'format': 'json'}
        rtdata = self.responced(url, data)
        return rtdata['status']['message']

    def mydomain(self):
        url = '%sDomain.List'%self.url
        data = {'user_token': self.usertoken, 'format': 'json'}
        rtdata = self.responced(url,data)
        domains = rtdata['domains']

        for domain in domains:
            if domain['name'] == self.domain:
                return domain['id']

    def record(self):
        url = '%sRecord.List' % self.url
        data = {'user_token': self.usertoken, 'format': 'json', 'domain_id':self.domainid}
        # print 'domain id = ',self.domainid
        rtdata = self.responced(url, data)
        return rtdata

    def checkrecord(self,host):
        # "True : exists host record\nFalse : non exists host record"
        record_datas = self.record()['records']
        for record_data in record_datas:
            if record_data['name'] != host:
                check = {'status':False, "recordid":'none'}
            else:

                check = {'status':True, "recordid":record_data['id']}
                break
        return check


    def addrecord(self, host):
        if self.checkrecord(host)['status']:
            return 'already exists "%s" record'%host
        else:
            url = '%sRecord.Create' % self.url
            data = {'user_token': self.usertoken, 'format': 'json', 'domain_id': self.domainid,'sub_domain':host,'record_type':self.record_type,'record_line':self.record_line,'value':self.serverip,'ttl':self.ttl}
            rtdata = self.responced(url, data)
            return rtdata

    def removerecord(self, host):
        checked = self.checkrecord(host)
        if checked['status']:
            url = '%sRecord.Remove' % self.url
            data = {'user_token': self.usertoken, 'format': 'json', 'domain_id': self.domainid,'record_id':checked['recordid'],}
            rtdata = self.responced(url, data)
            return rtdata
        else:
            return "There is no '%s' record"%host


    def responced(self,url,data):
        api = requests.post(url, data=data)
        return json.loads(api.text)






