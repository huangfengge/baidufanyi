# -*- coding: utf-8 -*-
# 第二步：进行翻译
jsCode = """
    function a(r) {
        if (Array.isArray(r)) {
            for (var o = 0, t = Array(r.length); o < r.length; o++)
                t[o] = r[o];
            return t
        }
        return Array.from(r)
    }
    function n(r, o) {
        for (var t = 0; t < o.length - 2; t += 3) {
            var a = o.charAt(t + 2);
            a = a >= "a" ? a.charCodeAt(0) - 87 : Number(a),
            a = "+" === o.charAt(t + 1) ? r >>> a : r << a,
            r = "+" === o.charAt(t) ? r + a & 4294967295 : r ^ a
        }
        return r
    }
    var i = null;
    function e(r) {
        var t = r.length;
        t > 30 && (r = "" + r.substr(0, 10) + r.substr(Math.floor(t / 2) - 5, 10) + r.substr(-10, 10))

        var u = void 0, l = "" + String.fromCharCode(103) + String.fromCharCode(116) + String.fromCharCode(107);

        u = null !== i ? i : (i = '320305.131321201' || "") || "";
        for (var d = u.split("."), m = Number(d[0]) || 0, s = Number(d[1]) || 0, S = [], c = 0, v = 0; v < r.length; v++) {
            var A = r.charCodeAt(v);
            128 > A ? S[c++] = A : (2048 > A ? S[c++] = A >> 6 | 192 : (55296 === (64512 & A) && v + 1 < r.length && 56320 === (64512 & r.charCodeAt(v + 1)) ? (A = 65536 + ((1023 & A) << 10) + (1023 & r.charCodeAt(++v)),
            S[c++] = A >> 18 | 240,
            S[c++] = A >> 12 & 63 | 128) : S[c++] = A >> 12 | 224,
            S[c++] = A >> 6 & 63 | 128),
            S[c++] = 63 & A | 128)
        }
        for (var p = m, F = "" + String.fromCharCode(43) + String.fromCharCode(45) + String.fromCharCode(97) + ("" + String.fromCharCode(94) + String.fromCharCode(43) + String.fromCharCode(54)), D = "" + String.fromCharCode(43) + String.fromCharCode(45) + String.fromCharCode(51) + ("" + String.fromCharCode(94) + String.fromCharCode(43) + String.fromCharCode(98)) + ("" + String.fromCharCode(43) + String.fromCharCode(45) + String.fromCharCode(102)), b = 0; b < S.length; b++)
            p += S[b],
            p = n(p, F);
        return p = n(p, D),
        p ^= s,
        0 > p && (p = (2147483647 & p) + 2147483648),
        p %= 1e6,
        p.toString() + "." + (p ^ m)
    }
"""
import execjs
from aip import AipSpeech
import time
import requests
import json

""" 你的 APPID AK SK """
APP_ID = '你的appID'
API_KEY = '你的key'
SECRET_KEY = '你的secret'

client = AipSpeech(APP_ID, API_KEY, SECRET_KEY)

# 第一步：读取文件
def get_file_content(filePath):
    with open(filePath, 'rb') as fp:
        return fp.read()

# 识别本地文件
respone = client.asr(get_file_content('16k.pcm'), 'pcm', 16000, {
    'dev_pid': 1536,
})

str = respone["result"][0]
print(str)
time1 = time.time()



sign = execjs.compile(jsCode).call("e", str)
print("sign:",sign)


data = {
    "from": "zh",
    "to": "en",
    "query":str,
    "transtype": "translang",
    "simple_means_flag": "3",
    "sign": sign,
    "token": "261d93745136045b7ea6a1badc54a075",
}
headers = {
    'Accept': '*/*',
    'Accept-Language': 'zh-CN,zh;q=0.9',
    'Connection': 'keep-alive',
    'Content-Type': 'application/x-www-form-urlencoded; charset=UTF-8',
    'Cookie': 'BAIDUID=BADB5E5D72667C5F54A34848245C74AB:FG=1; BIDUPSID=BADB5E5D72667C5F54A34848245C74AB; PSTM=1534988617; H_WISE_SIDS=124614_125822_108269_124694_125945_114745_123552_125629_120162_125735_118894_118867_118845_118820_118788_107311_125006_124978_117428_125776_125652_124636_124897_124939_125487_125171_125289_125710_125285_124938_124683_124030_110085_123289_125645_125428_125875; REALTIME_TRANS_SWITCH=1; FANYI_WORD_SWITCH=1; HISTORY_SWITCH=1; SOUND_SPD_SWITCH=1; SOUND_PREFER_SWITCH=1; BDUSS=0N6UHl4VTVQR1k2d0k1YWtMRmRRaTJhLWFpQk1mLWZwS3YxU01UTW0tMEtNaEpjQVFBQUFBJCQAAAAAAAAAAAEAAACrlRIPNDM0ODkwOTc0AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAql6lsKpepbS; Hm_lvt_64ecd82404c51e03dc91cb9e8c025574=1542288873,1542333802,1542365440,1542422410; __cfduid=dde3763f9db2ea63b180aa1372d14e3921542882903; BDSFRCVID=LNuOJeC62mNCkaT90LMd--Y9EeTt9t7TH6aItwTAQU9bjkQNDx7JEG0PfU8g0KubJmOPogKKKgOTHICF_2uxOjjg8UtVJeC6EG0P3J; H_BDCLCKID_SF=tR-tVC0-fCI3fP36q4co-4FjbqOH54cXHDOH0Kvzt-55OR5Jj65NW-FtMGtf2j3kyCT-hInH-RF5jDn-3MA--fF1hmcayhjMXbvXQp-2KhTCsq0x0-6le-bQypoa2b-HBIOMahvc5h7xOhIC05C-jTvBDG_eq-JQ2C7WsJjs24ThD6rnhPF3K4rbKP6-3MJO3b7zon6lJbrSSpO_5n3V3tkEKh6tb6oP-N7CohFLtC0BhK0Gj6Rjh-40b2TJK4625CoJsJOOaC7tqR7zKPnhK4t8HfJfhtJfHJue_II5JjuKhI-wjTD_D6J3eHtqqh5gW57Z0lOnMp05jt3uh6J_hKurXpKJ0JQ2aCDfhC5RLKOSVIO_e6LbejOWjHDs5-7ybCPXLn58Kb5_f5rnhPF3yMvDKP6-3MJO3b7JbpTvJbrSOUob-jrl5f63247XtbTwKRnlohFLtD8KMI-GjjRMK4_SMUoHetrK-D5XQbC8Kb7VbTC4LfnkbfJBD4-jXJJC2JvXQpOy5R5lfM7NyUOO5fI7yajK25KHbe5h2h5v3fOASIO63bbpQT8rbfDOK5Oib4j7htOnab3vOpvTXpO1yftzBN5thURB2DkO-4bCWJ5TMl5jDh05y6TXjH-ttTLDJn3fL-082R6oDn8k-PnVePF3LtnZKxtqtDDOBDJ7QI3lDbk6557ZbbDqhRJ8t4TnWncKWho1bIb_MR5cK65IhPrb2HJ405OT2j-O0KJcbRo0Hpb4hPJvyUPsXnO7BxJlXbrtXp7_2J0WStbKy4oTjxL1Db0eBjIHtnFJVI05fCvKeJbYK4oj5KCyMfca5C6JKCOa3RA8Kb7VbpR85fnkbfJBDxcw3-nKQnKj2p6eH66Zbq5N3b8B55-7yajKBR3fQg6I-CTt2JvG8lb9jU6pQT8rbR_OK5OibCrZWl7xab3vOpvTXpO1yftzBN5thURB2DkO-4bCWJ5TMl5jDh05y6ISKx-_J5te3f; MCITY=-%3A; H_PS_PSSID=1438_21081_18559_28019_26350; BDORZ=B490B5EBF6F3CD402E515D22BCDA1598; delPer=0; PSINO=5; locale=zh; Hm_lpvt_64ecd82404c51e03dc91cb9e8c025574=1544166294; from_lang_often=%5B%7B%22value%22%3A%22dan%22%2C%22text%22%3A%22%u4E39%u9EA6%u8BED%22%7D%2C%7B%22value%22%3A%22en%22%2C%22text%22%3A%22%u82F1%u8BED%22%7D%2C%7B%22value%22%3A%22zh%22%2C%22text%22%3A%22%u4E2D%u6587%22%7D%5D; to_lang_often=%5B%7B%22value%22%3A%22zh%22%2C%22text%22%3A%22%u4E2D%u6587%22%7D%2C%7B%22value%22%3A%22en%22%2C%22text%22%3A%22%u82F1%u8BED%22%7D%5D',
    'Host': 'fanyi.baidu.com',
    'Origin': 'https://fanyi.baidu.com',
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/68.0.3440.106 Safari/537.36',
}
url = 'https://fanyi.baidu.com/v2transapi'
res = requests.post(url=url, headers=headers, data=data)

result = json.loads(res.text).get("trans_result").get('data')[0]
item = dict()
item= result.get('dst')

str = item
print(str) #{'汉语中文': 'Chinese language'}
time2 = time.time()
print("语音合成文字：",time2-time1)



client = AipSpeech(APP_ID, API_KEY, SECRET_KEY)
time3 = time.time()

re = client.synthesis(str, 'zh', 1, {
    'vol': 5,  # 音量
    'spd': 4,  # 语速
    'pit': 9,  # 语调
    'per': 0,  # 0：女 1：男 3：逍遥 4：小萝莉
})
# 识别正确返回语音二进制 错误则返回dict 参照下面错误码
if not isinstance(re, dict):
    with open('jjjj.wav', 'wb') as f:
        f.write(re)

time4 = time.time()
print("文字合成语音",time4-time3)
