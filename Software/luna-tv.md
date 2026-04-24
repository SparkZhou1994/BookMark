# luna-tv
## install[docker-compose.yml]
```
services:
  moontv-core:
    image: ghcr.io/moontechlab/lunatv:latest
    container_name: moontv-core
    restart: on-failure
    ports:
      - '3000:3000'
    environment:
      - USERNAME=admin
      - PASSWORD=admin_password_123
      - NEXT_PUBLIC_STORAGE_TYPE=upstash
      - UPSTASH_URL=https://console.upstash.com/
      - UPSTASH_TOKEN=https://console.upstash.com/
```
## source
### source[https://gist.githubusercontent.com/senshinya/5a5cb900dfa888fd61d767530f00fc48/raw/gistfile1.txt]
```
{
    "cache_time": 7200,
    "api_site": {
        "dyttzy": {
            "api": "http://caiji.dyttzyapi.com/api.php/provide/vod",
            "name": "电影天堂",
            "detail": "http://caiji.dyttzyapi.com"
        },
        "bfzy": {
            "api": "https://bfzyapi.com/api.php/provide/vod",
            "name": "暴风资源"
        },
        "tyyszy": {
            "api": "https://tyyszy.com/api.php/provide/vod",
            "name": "天涯资源"
        },
        "ffzy": {
            "api": "https://api.ffzyapi.com/api.php/provide/vod",
            "name": "非凡影视",
            "detail": "http://ffzy5.tv"
        },
        "zy360": {
            "api": "https://360zy.com/api.php/provide/vod",
            "name": "360资源"
        },
        "maotaizy": {
            "api": "https://caiji.maotaizy.cc/api.php/provide/vod",
            "name": "茅台资源"
        },
        "wolong": {
            "api": "https://wolongzyw.com/api.php/provide/vod",
            "name": "卧龙资源"
        },
        "jisu": {
            "api": "https://jszyapi.com/api.php/provide/vod",
            "name": "极速资源",
            "detail": "https://jszyapi.com"
        },
        "dbzy": {
            "api": "https://dbzy.tv/api.php/provide/vod",
            "name": "豆瓣资源"
        },
        "mozhua": {
            "api": "https://mozhuazy.com/api.php/provide/vod",
            "name": "魔爪资源"
        },
        "mdzy": {
            "api": "https://www.mdzyapi.com/api.php/provide/vod",
            "name": "魔都资源"
        },
        "zuid": {
            "api": "https://api.zuidapi.com/api.php/provide/vod",
            "name": "最大资源"
        },
        "yinghua": {
            "api": "https://m3u8.apiyhzy.com/api.php/provide/vod",
            "name": "樱花资源"
        },
        "wujin": {
            "api": "https://api.wujinapi.me/api.php/provide/vod",
            "name": "无尽资源"
        },
        "wwzy": {
            "api": "https://wwzy.tv/api.php/provide/vod",
            "name": "旺旺短剧"
        },
        "ikun": {
            "api": "https://ikunzyapi.com/api.php/provide/vod",
            "name": "iKun资源"
        },
        "lzi": {
            "api": "https://cj.lziapi.com/api.php/provide/vod",
            "name": "量子资源"
        },
        "bdzy": {
            "api": "https://api.apibdzy.com/api.php/provide/vod",
            "name": "百度资源"
        },
        "hongniuzy": {
            "api": "https://www.hongniuzy2.com/api.php/provide/vod",
            "name": "红牛资源"
        },
        "xinlangaa": {
            "api": "https://api.xinlangapi.com/xinlangapi.php/provide/vod",
            "name": "新浪资源"
        },
        "ckzy": {
            "api": "https://ckzy.me/api.php/provide/vod",
            "name": "CK资源",
            "detail": "https://ckzy.me"
        },
        "ukuapi": {
            "api": "https://api.ukuapi.com/api.php/provide/vod",
            "name": "U酷资源",
            "detail": "https://api.ukuapi.com"
        },
        "1080zyk": {
            "api": "https://api.1080zyku.com/inc/apijson.php/",
            "name": "1080资源",
            "detail": "https://api.1080zyku.com"
        },
        "hhzyapi": {
            "api": "https://hhzyapi.com/api.php/provide/vod",
            "name": "豪华资源",
            "detail": "https://hhzyapi.com"
        },
        "subocaiji": {
            "api": "https://subocaiji.com/api.php/provide/vod",
            "name": "速博资源"
        },
        "p2100": {
            "api": "https://p2100.net/api.php/provide/vod",
            "name": "飘零资源",
            "detail": "https://p2100.net"
        }
    },
    "lives": {
        "yang-1989": {
            "name": "YanG",
            "url": "https://tv-1.iill.top/m3u/Gather"
        },
        "Mursor": {
            "name": "Mursor",
            "url": "https://sub.ottiptv.cc/iptv.m3u",
            "ua": "okHttp/Mod-1.2.0"
        }
    }
}
```
### source[https://tv2.910501.xyz/api/tvbox/config?format=json&mode=fast]
```
{"spider":"https://gitcode.net/qq_26898231/TVBox/-/raw/main/JAR/XC.jar;md5;e53eb37c4dc3dce1c8ee0c996ca3a024","sites":[{"key":"dyttzy","name":"电影天堂","type":1,"api":"http://caiji.dyttzyapi.com/api.php/provide/vod/?ac=list","searchable":1,"quickSearch":1,"filterable":1,"changeable":1,"header":{"User-Agent":"Mozilla/5.0 (Linux; Android 11) AppleWebKit/537.36","Connection":"close"},"ext":"http://caiji.dyttzyapi.com"},{"key":"bfzy","name":"暴风资源","type":1,"api":"https://bfzyapi.com/api.php/provide/vod/?ac=list","searchable":1,"quickSearch":1,"filterable":1,"changeable":1,"header":{"User-Agent":"Mozilla/5.0 (Linux; Android 11) AppleWebKit/537.36","Connection":"close"},"ext":""},{"key":"tyyszy","name":"天涯资源","type":1,"api":"https://tyyszy.com/api.php/provide/vod/?ac=list","searchable":1,"quickSearch":1,"filterable":1,"changeable":1,"header":{"User-Agent":"Mozilla/5.0 (Linux; Android 11) AppleWebKit/537.36","Connection":"close"},"ext":""},{"key":"ffzy","name":"非凡影视","type":1,"api":"https://api.ffzyapi.com/api.php/provide/vod/?ac=list","searchable":1,"quickSearch":1,"filterable":1,"changeable":1,"header":{"User-Agent":"Mozilla/5.0 (Linux; Android 11) AppleWebKit/537.36","Connection":"close"},"ext":"http://ffzy5.tv"},{"key":"zy360","name":"360资源","type":1,"api":"https://360zy.com/api.php/provide/vod/?ac=list","searchable":1,"quickSearch":1,"filterable":1,"changeable":1,"header":{"User-Agent":"Mozilla/5.0 (Linux; Android 11) AppleWebKit/537.36","Connection":"close"},"ext":""},{"key":"maotaizy","name":"茅台资源","type":1,"api":"https://caiji.maotaizy.cc/api.php/provide/vod/?ac=list","searchable":1,"quickSearch":1,"filterable":1,"changeable":1,"header":{"User-Agent":"Mozilla/5.0 (Linux; Android 11) AppleWebKit/537.36","Connection":"close"},"ext":""},{"key":"wolong","name":"卧龙资源","type":1,"api":"https://wolongzyw.com/api.php/provide/vod/?ac=list","searchable":1,"quickSearch":1,"filterable":1,"changeable":1,"header":{"User-Agent":"Mozilla/5.0 (Linux; Android 11) AppleWebKit/537.36","Connection":"close"},"ext":""},{"key":"jisu","name":"极速资源","type":1,"api":"https://jszyapi.com/api.php/provide/vod/?ac=list","searchable":1,"quickSearch":1,"filterable":1,"changeable":1,"header":{"User-Agent":"Mozilla/5.0 (Linux; Android 11) AppleWebKit/537.36","Connection":"close"},"ext":"https://jszyapi.com"},{"key":"dbzy","name":"豆瓣资源","type":1,"api":"https://dbzy.tv/api.php/provide/vod/?ac=list","searchable":1,"quickSearch":1,"filterable":1,"changeable":1,"header":{"User-Agent":"Mozilla/5.0 (Linux; Android 11) AppleWebKit/537.36","Connection":"close"},"ext":""},{"key":"mozhua","name":"魔爪资源","type":1,"api":"https://mozhuazy.com/api.php/provide/vod/?ac=list","searchable":1,"quickSearch":1,"filterable":1,"changeable":1,"header":{"User-Agent":"Mozilla/5.0 (Linux; Android 11) AppleWebKit/537.36","Connection":"close"},"ext":""},{"key":"mdzy","name":"魔都资源","type":1,"api":"https://www.mdzyapi.com/api.php/provide/vod/?ac=list","searchable":1,"quickSearch":1,"filterable":1,"changeable":1,"header":{"User-Agent":"Mozilla/5.0 (Linux; Android 11) AppleWebKit/537.36","Connection":"close"},"ext":""},{"key":"zuid","name":"最大资源","type":1,"api":"https://api.zuidapi.com/api.php/provide/vod/?ac=list","searchable":1,"quickSearch":1,"filterable":1,"changeable":1,"header":{"User-Agent":"Mozilla/5.0 (Linux; Android 11) AppleWebKit/537.36","Connection":"close"},"ext":""},{"key":"yinghua","name":"樱花资源","type":1,"api":"https://m3u8.apiyhzy.com/api.php/provide/vod/?ac=list","searchable":1,"quickSearch":1,"filterable":1,"changeable":1,"header":{"User-Agent":"Mozilla/5.0 (Linux; Android 11) AppleWebKit/537.36","Connection":"close"},"ext":""},{"key":"wujin","name":"无尽资源","type":1,"api":"https://api.wujinapi.me/api.php/provide/vod/?ac=list","searchable":1,"quickSearch":1,"filterable":1,"changeable":1,"header":{"User-Agent":"Mozilla/5.0 (Linux; Android 11) AppleWebKit/537.36","Connection":"close"},"ext":""},{"key":"wwzy","name":"旺旺短剧","type":1,"api":"https://wwzy.tv/api.php/provide/vod/?ac=list","searchable":1,"quickSearch":1,"filterable":1,"changeable":1,"header":{"User-Agent":"Mozilla/5.0 (Linux; Android 11) AppleWebKit/537.36","Connection":"close"},"ext":""},{"key":"ikun","name":"iKun资源","type":1,"api":"https://ikunzyapi.com/api.php/provide/vod/?ac=list","searchable":1,"quickSearch":1,"filterable":1,"changeable":1,"header":{"User-Agent":"Mozilla/5.0 (Linux; Android 11) AppleWebKit/537.36","Connection":"close"},"ext":""},{"key":"lzi","name":"量子资源","type":1,"api":"https://cj.lziapi.com/api.php/provide/vod/?ac=list","searchable":1,"quickSearch":1,"filterable":1,"changeable":1,"header":{"User-Agent":"Mozilla/5.0 (Linux; Android 11) AppleWebKit/537.36","Connection":"close"},"ext":""},{"key":"bdzy","name":"百度资源","type":1,"api":"https://api.apibdzy.com/api.php/provide/vod/?ac=list","searchable":1,"quickSearch":1,"filterable":1,"changeable":1,"header":{"User-Agent":"Mozilla/5.0 (Linux; Android 11) AppleWebKit/537.36","Connection":"close"},"ext":""},{"key":"hongniuzy","name":"红牛资源","type":1,"api":"https://www.hongniuzy2.com/api.php/provide/vod/?ac=list","searchable":1,"quickSearch":1,"filterable":1,"changeable":1,"header":{"User-Agent":"Mozilla/5.0 (Linux; Android 11) AppleWebKit/537.36","Connection":"close"},"ext":""},{"key":"xinlangaa","name":"新浪资源","type":1,"api":"https://api.xinlangapi.com/xinlangapi.php/provide/vod/?ac=list","searchable":1,"quickSearch":1,"filterable":1,"changeable":1,"header":{"User-Agent":"Mozilla/5.0 (Linux; Android 11) AppleWebKit/537.36","Connection":"close"},"ext":""},{"key":"ckzy","name":"CK资源","type":1,"api":"https://ckzy.me/api.php/provide/vod/?ac=list","searchable":1,"quickSearch":1,"filterable":1,"changeable":1,"header":{"User-Agent":"Mozilla/5.0 (Linux; Android 11) AppleWebKit/537.36","Connection":"close"},"ext":"https://ckzy.me"},{"key":"ukuapi","name":"U酷资源","type":1,"api":"https://api.ukuapi.com/api.php/provide/vod/?ac=list","searchable":1,"quickSearch":1,"filterable":1,"changeable":1,"header":{"User-Agent":"Mozilla/5.0 (Linux; Android 11) AppleWebKit/537.36","Connection":"close"},"ext":"https://api.ukuapi.com"},{"key":"1080zyk","name":"1080资源","type":1,"api":"https://api.1080zyku.com/inc/apijson.php/?ac=list","searchable":1,"quickSearch":1,"filterable":1,"changeable":1,"header":{"User-Agent":"Mozilla/5.0 (Linux; Android 11) AppleWebKit/537.36","Connection":"close"},"ext":"https://api.1080zyku.com"},{"key":"hhzyapi","name":"豪华资源","type":1,"api":"https://hhzyapi.com/api.php/provide/vod/?ac=list","searchable":1,"quickSearch":1,"filterable":1,"changeable":1,"header":{"User-Agent":"Mozilla/5.0 (Linux; Android 11) AppleWebKit/537.36","Connection":"close"},"ext":"https://hhzyapi.com"},{"key":"subocaiji","name":"速博资源","type":1,"api":"https://subocaiji.com/api.php/provide/vod/?ac=list","searchable":1,"quickSearch":1,"filterable":1,"changeable":1,"header":{"User-Agent":"Mozilla/5.0 (Linux; Android 11) AppleWebKit/537.36","Connection":"close"},"ext":""},{"key":"p2100","name":"飘零资源","type":1,"api":"https://p2100.net/api.php/provide/vod/?ac=list","searchable":1,"quickSearch":1,"filterable":1,"changeable":1,"header":{"User-Agent":"Mozilla/5.0 (Linux; Android 11) AppleWebKit/537.36","Connection":"close"},"ext":"https://p2100.net"},{"key":"aqyzy","name":"爱奇艺","type":1,"api":"https://iqiyizyapi.com/api.php/provide/vod/?ac=list","searchable":1,"quickSearch":1,"filterable":1,"changeable":1,"header":{"User-Agent":"Mozilla/5.0 (Linux; Android 11) AppleWebKit/537.36","Connection":"close"},"ext":"https://iqiyizyapi.com"},{"key":"yzzy","name":"优质资源","type":1,"api":"https://api.yzzy-api.com/inc/apijson.php/?ac=list","searchable":1,"quickSearch":1,"filterable":1,"changeable":1,"header":{"User-Agent":"Mozilla/5.0 (Linux; Android 11) AppleWebKit/537.36","Connection":"close"},"ext":""},{"key":"myzy","name":"猫眼资源","type":1,"api":"https://api.maoyanapi.top/api.php/provide/vod/?ac=list","searchable":1,"quickSearch":1,"filterable":1,"changeable":1,"header":{"User-Agent":"Mozilla/5.0 (Linux; Android 11) AppleWebKit/537.36","Connection":"close"},"ext":"https://www.maoyanzy.com"},{"key":"rycj","name":"如意资源","type":1,"api":"https://cj.rycjapi.com/api.php/provide/vod/?ac=list","searchable":1,"quickSearch":1,"filterable":1,"changeable":1,"header":{"User-Agent":"Mozilla/5.0 (Linux; Android 11) AppleWebKit/537.36","Connection":"close"},"ext":""},{"key":"jinyingzy","name":"金鹰点播","type":1,"api":"https://jinyingzy.com/api.php/provide/vod/?ac=list","searchable":1,"quickSearch":1,"filterable":1,"changeable":1,"header":{"User-Agent":"Mozilla/5.0 (Linux; Android 11) AppleWebKit/537.36","Connection":"close"},"ext":"https://jinyingzy.com"},{"key":"guangsuapi","name":"光速资源","type":1,"api":"https://api.guangsuapi.com/api.php/provide/vod/?ac=list","searchable":1,"quickSearch":1,"filterable":1,"changeable":1,"header":{"User-Agent":"Mozilla/5.0 (Linux; Android 11) AppleWebKit/537.36","Connection":"close"},"ext":"https://api.guangsuapi.com"}],"lives":[],"parses":[{"name":"极速解析","type":0,"url":"https://jx.xmflv.com/?url=","ext":{"flag":["all"]}},{"name":"Json并发","type":2,"url":"Parallel"}],"flags":["youku","qq","iqiyi","qiyi","letv","sohu","mgtv"],"wallpaper":"","maxHomeVideoContent":"15","spider_url":"fallback","spider_md5":"f6da9ab71e4a104eb3f9f7c9ab162b22","spider_cached":true,"spider_real_size":621,"spider_tried":14,"spider_success":false,"spider_backup":"https://gitcode.net/qq_26898231/TVBox/-/raw/main/JAR/XC.jar","spider_candidates":["https://deco-spider.oss-cn-hangzhou.aliyuncs.com/XC.jar","https://deco-spider-1250000000.cos.ap-shanghai.myqcloud.com/XC.jar","https://cdn.gitcode.net/qq_26898231/TVBox/-/raw/main/JAR/XC.jar","https://cdn.gitee.com/q215613905/TVBoxOS/raw/main/JAR/XC.jar","https://gitcode.net/qq_26898231/TVBox/-/raw/main/JAR/XC.jar","https://gitee.com/q215613905/TVBoxOS/raw/main/JAR/XC.jar","https://gitcode.net/qq_26898231/TVBox/-/raw/main/JAR/XC.jar","https://ghproxy.com/https://raw.githubusercontent.com/FongMi/CatVodSpider/main/jar/custom_spider.jar"]}
```
### source-merge
```
{
    "cache_time": 7200,
    "api_site": {
        "dyttzy": {
            "api": "http://caiji.dyttzyapi.com/api.php/provide/vod",
            "name": "电影天堂",
            "detail": "http://caiji.dyttzyapi.com"
        },
        "bfzy": {
            "api": "https://bfzyapi.com/api.php/provide/vod",
            "name": "暴风资源"
        },
        "tyyszy": {
            "api": "https://tyyszy.com/api.php/provide/vod",
            "name": "天涯资源"
        },
        "ffzy": {
            "api": "https://api.ffzyapi.com/api.php/provide/vod",
            "name": "非凡影视",
            "detail": "http://ffzy5.tv"
        },
        "zy360": {
            "api": "https://360zy.com/api.php/provide/vod",
            "name": "360资源"
        },
        "maotaizy": {
            "api": "https://caiji.maotaizy.cc/api.php/provide/vod",
            "name": "茅台资源"
        },
        "wolong": {
            "api": "https://wolongzyw.com/api.php/provide/vod",
            "name": "卧龙资源"
        },
        "jisu": {
            "api": "https://jszyapi.com/api.php/provide/vod",
            "name": "极速资源",
            "detail": "https://jszyapi.com"
        },
        "dbzy": {
            "api": "https://dbzy.tv/api.php/provide/vod",
            "name": "豆瓣资源"
        },
        "mozhua": {
            "api": "https://mozhuazy.com/api.php/provide/vod",
            "name": "魔爪资源"
        },
        "mdzy": {
            "api": "https://www.mdzyapi.com/api.php/provide/vod",
            "name": "魔都资源"
        },
        "zuid": {
            "api": "https://api.zuidapi.com/api.php/provide/vod",
            "name": "最大资源"
        },
        "yinghua": {
            "api": "https://m3u8.apiyhzy.com/api.php/provide/vod",
            "name": "樱花资源"
        },
        "wujin": {
            "api": "https://api.wujinapi.me/api.php/provide/vod",
            "name": "无尽资源"
        },
        "wwzy": {
            "api": "https://wwzy.tv/api.php/provide/vod",
            "name": "旺旺短剧"
        },
        "ikun": {
            "api": "https://ikunzyapi.com/api.php/provide/vod",
            "name": "iKun资源"
        },
        "lzi": {
            "api": "https://cj.lziapi.com/api.php/provide/vod",
            "name": "量子资源"
        },
        "bdzy": {
            "api": "https://api.apibdzy.com/api.php/provide/vod",
            "name": "百度资源"
        },
        "hongniuzy": {
            "api": "https://www.hongniuzy2.com/api.php/provide/vod",
            "name": "红牛资源"
        },
        "xinlangaa": {
            "api": "https://api.xinlangapi.com/xinlangapi.php/provide/vod",
            "name": "新浪资源"
        },
        "ckzy": {
            "api": "https://ckzy.me/api.php/provide/vod",
            "name": "CK资源",
            "detail": "https://ckzy.me"
        },
        "ukuapi": {
            "api": "https://api.ukuapi.com/api.php/provide/vod",
            "name": "U酷资源",
            "detail": "https://api.ukuapi.com"
        },
        "1080zyk": {
            "api": "https://api.1080zyku.com/inc/apijson.php/",
            "name": "1080资源",
            "detail": "https://api.1080zyku.com"
        },
        "hhzyapi": {
            "api": "https://hhzyapi.com/api.php/provide/vod",
            "name": "豪华资源",
            "detail": "https://hhzyapi.com"
        },
        "subocaiji": {
            "api": "https://subocaiji.com/api.php/provide/vod",
            "name": "速博资源"
        },
        "p2100": {
            "api": "https://p2100.net/api.php/provide/vod",
            "name": "飘零资源",
            "detail": "https://p2100.net"
        },
    "ruyi": {
      "api": "http://cj.rycjapi.com/api.php/provide/vod",
      "name": "如意资源"
    },
    "xiaomaomi": {
      "api": "https://zy.xmm.hk/api.php/provide/vod",
      "name": "小猫咪资源"
    },
    "heimuer": {
      "api": "https://json.heimuer.xyz/api.php/provide/vod",
      "name": "黑木耳",
      "detail": "https://heimuer.tv"
    },
    "huaweibavod": {
      "api": "https://hw8.live/api.php/provide/vod/",
      "name": "华为吧|点播"
    },
    "fujuvod": {
      "api": "http://www.fuju2024.cc:8013/ruifenglb_api.php/provide/vod/",
      "name": "腐剧|点播"
    },
    "jinyingvod": {
      "api": "https://jinyingzy.com/api.php/provide/vod/",
      "name": "金鹰|点播"
    },
    "okvod": {
      "api": "https://okzyw9.com/api.php/provide/vod/",
      "name": "OK|点播"
    },
    "baopianvod": {
      "api": "https://zpsps.com/api.php/provide/vod/",
      "name": "宝片|点播"
    },
    "xiaohuangrenvod": {
      "api": "https://iqyi.xiaohuangrentv.com/api.php/provide/vod/",
      "name": "小黄人|点播"
    },
    "niuniuvod": {
      "api": "https://api.niuniuzy.me/api.php/provide/vod/",
      "name": "牛牛|点播"
    },
    "yayavod": {
      "api": "https://cj.yayazy.net/api.php/provide/vod/",
      "name": "丫丫|点播"
    },
    "yingtuvod": {
      "api": "https://cj.vodimg.top/api.php/provide/vod/",
      "name": "影图|点播"
    },
    "ukuvod": {
      "api": "https://api.ukuapi.com/api.php/provide/vod/",
      "name": "U酷|点播"
    },
    "haohuavod": {
      "api": "https://hhzyapi.com/api.php/provide/vod",
      "name": "豪华|点播"
    },
    "suonivod": {
      "api": "https://suoniapi.com/api.php/provide/vod/",
      "name": "索尼|点播"
    },
    "baofengvod": {
      "api": "https://bfzyapi.com/api.php/provide/vod/",
      "name": "暴风|点播"
    },
    "hongniuvod": {
      "api": "https://www.hongniuzy2.com/api.php/provide/vod/",
      "name": "红牛|点播"
    },
    "kuaichevod": {
      "api": "https://caiji.kczyapi.com/api.php/provide/vod/from/kcm3u8/",
      "name": "快车|点播"
    },
    "shandianvod": {
      "api": "http://sdzyapi.com/api.php/provide/vod/",
      "name": "闪电|点播"
    },
    "wolongvod": {
      "api": "https://collect.wolongzyw.com/api.php/provide/vod/",
      "name": "卧龙|点播"
    },
    "huyavod": {
      "api": "https://www.huyaapi.com/api.php/provide/vod/",
      "name": "虎牙|点播"
    },
    "piaolingvod": {
      "api": "https://p2100.net/api.php/provide/vod/",
      "name": "飘零|点播"
    },
    "subovod": {
      "api": "https://subocaiji.com/api.php/provide/vod/",
      "name": "速博|点播"
    },
    "moduvod": {
      "api": "https://caiji.moduapi.cc/api.php/provide/vod/",
      "name": "魔都|点播"
    },
    "zuidadianbo": {
      "api": "http://zuidazy.me/api.php/provide/vod/",
      "name": "最大|点播"
    }
    },
    "lives": {
        "yang-1989": {
            "name": "YanG",
            "url": "https://tv-1.iill.top/m3u/Gather"
        },
        "Mursor": {
            "name": "Mursor",
            "url": "https://sub.ottiptv.cc/iptv.m3u",
            "ua": "okHttp/Mod-1.2.0"
        }
    }
}
```

# XMBOX[https://github.com/Tosencen/XMBOX]
## source
### 主力[https://16409.kstore.vip/tv/ngzmods.json]
```
{
  "spider": "http://192.140.163.220:9986/svip/spider.jar",
  "wallpaper": "https://cn.bing.com/th?id=OHR.RomeView_ZH-CN5882212305_1920x1080.jpg",
  "sites": [
    {
      "key": "qiao4k1",
      "name": "☯️微信公众号：北方学习天地(关注不迷路)",
      "type": 3,
      "api": "csp_qiao2",
      "playerType": 2,
      "jar": "https://16409.kstore.vip/tv/flower.jar",
      "ext": "7lj763gg402i7942412h849jg1558j9jl8g19213il86316k465719lik158491h179312h4934183ig2l05g9"
    },
    {
      "key": "qiao4k2",
      "name": "☯️4K在线┃NGZ至臻",
      "type": 3,
      "api": "csp_Qiji",
      "playerType": 2,
      "jar": "https://16409.kstore.vip/tv/flower.jar",
      "ext": "7lj763gg402i7942553k9hj9lk5h8h86k4gg845kili63835585155k7k103470h1k955lhk9706ih80280ikij0g7l91jlhl565775ikjhj9lg422962ig23ili4845k6g492l0"
    },
    {
      "key": "移动",
      "name": "☯️移动┃在线4K",
      "type": 3,
      "playerType": 1,
      "api": "csp_YD",
      "searchable": 1,
      "quickSearch": 1
    },
    {
      "key": "配置中心",
      "name": "🗂个人云盘┃配置",
      "type": 3,
      "api": "csp_Config",
      "searchable": 0,
      "changeable": 0,
      "style": {
        "type": "rect",
        "ratio": 1.597
      }
    },
    {
      "key": "mihdr",
      "name": "📀P-A|4K😛",
      "type": 3,
      "jar": "http://139.224.213.240:60789/ZC4K.jar",
      "api": "csp_ZC4K",
      "ext": "YUhSMGNEb3ZMekV6T1M0eU1qUXVNakV6TGpJME1Eb3hNak14TWk5aGNHa3VjR2h3UDNOcGRHVTliV2xvWkhJakl5TnRhV2hrY2c9PQ=="
    },
    {
      "key": "ouge",
      "name": "📀P-B|4K😋",
      "type": 3,
      "jar": "http://139.224.213.240:60789/ZC4K.jar",
      "api": "csp_ZC4K",
      "ext": "YUhSMGNEb3ZMekV6T1M0eU1qUXVNakV6TGpJME1Eb3hNak14TWk5aGNHa3VjR2h3UDNOcGRHVTliM1ZuWlNNakkyOTFaMlU9"
    },
    {
      "key": "libo",
      "name": "📀P-C|4K😀",
      "type": 3,
      "jar": "http://139.224.213.240:60789/ZC4K.jar",
      "api": "csp_ZC4K",
      "ext": "YUhSMGNEb3ZMekV6T1M0eU1qUXVNakV6TGpJME1Eb3hNak14TWk5aGNHa3VjR2h3UDNOcGRHVTliR2xpYnlNakkyeHBZbTg9"
    },
    {
      "key": "labi",
      "name": "📀P-G|4K😘",
      "type": 3,
      "jar": "http://139.224.213.240:60789/ZC4K.jar",
      "api": "csp_ZC4K",
      "ext": "YUhSMGNEb3ZMekV6T1M0eU1qUXVNakV6TGpJME1Eb3hNak14TWk5aGNHa3VjR2h3UDNOcGRHVTliR0ZpYVNNakkyeGhZbWs9"
    },
    {
      "key": "yanshi1",
      "name": "📀P-E|4K💘",
      "type": 3,
      "jar": "http://123.207.224.130:7033/ZC4K.jar",
      "api": "csp_ZC4K",
      "ext": "YUhSMGNEb3ZMekV5TXk0eU1EY3VNakkwTGpFek1EbzNNRE16TDJGd2FTNXdhSEEvYzJsMFpUMXZkV2RsSXlNamIzVm5aUT09"
    },
    {
      "key": "瓜子影视",
      "name": "🍉GZ┃HD2K",
      "type": 3,
      "api": "csp_Gz360",
      "searchable": 1,
      "quickSearch": 1,
      "filterable": 1
    },
    {
      "key": "金牌影视",
      "name": "🔦金牌|HD2K",
      "type": 3,
      "api": "csp_Jpys",
      "ext": "https://m.hkybqufgh.com,https://m.sizhengxt.com,https://m.9zhoukj.com,https://m.sizhengxt.com,https://m.jiabaide.cn"
    },
    {
      "key": "热播影视",
      "name": "🏮热播｜影视",
      "type": 3,
      "api": "csp_AppRJ",
      "searchable": 1,
      "quickSearch": 1,
      "filterable": 0,
      "ext": {
        "url": "http://v.rbotv.cn"
      }
    },
    {
      "key": "农民影视",
      "name": "🏮农民｜秒播",
      "type": 3,
      "api": "csp_XYQHiker",
      "searchable": 1,
      "quickSearch": 1,
      "filterable": 1,
      "ext": "http://192.140.163.220:9986/svip/lib/农民影视.json"
    },
    {
      "key": "360zy",
      "name": "🏮飞龙｜秒播",
      "type": 1,
      "api": "https://360zy.com/api.php/provide/vod?",
      "searchable": 1,
      "changeable": 1,
      "categories": [
        "动作片",
        "喜剧片",
        "爱情片",
        "科幻片",
        "恐怖片",
        "剧情片",
        "战争片",
        "古装片",
        "悬疑片",
        "犯罪片",
        "灾难片",
        "国产剧",
        "香港剧",
        "韩国剧",
        "欧美剧",
        "台湾剧",
        "日本剧",
        "海外剧",
        "泰国剧",
        "大陆综艺",
        "港台综艺",
        "日韩综艺",
        "欧美综艺",
        "国产动漫",
        "欧美动漫",
        "日韩动漫",
        "现代都市",
        "脑洞悬疑",
        "年代穿越",
        "古装仙侠",
        "女频恋爱",
        "成长逆袭",
        "反转爽剧"
      ]
    },
    {
      "key": "三六零",
      "name": "🏮三六｜秒播",
      "type": 3,
      "api": "csp_SP360"
    },
    {
      "key": "紫金",
      "name": "🌸紫金丨APP",
      "type": 3,
      "quickSearch": 1,
      "api": "csp_AppGet",
      "ext": {
        "url": "http://www.zjcvod.com",
        "site": "",
        "dataKey": "ab4e9a421675f14b",
        "dataIv": "ab4e9a421675f14b",
        "deviceId": "",
        "version": "",
        "ua": ""
      }
    },
    {
      "key": "丫丫",
      "name": "🌸丫丫｜APP",
      "type": 3,
      "quickSearch": 1,
      "api": "csp_AppGet",
      "ext": {
        "url": "http://tv.yy-fun.cc",
        "dataKey": "qkxnwkfjwpcnwycl",
        "dataIv": "qkxnwkfjwpcnwycl",
        "deviceId": "",
        "version": ""
      }
    },
    {
      "key": "驿站",
      "name": "🌸驿站丨APP",
      "type": 3,
      "quickSearch": 1,
      "api": "csp_AppGet",
      "ext": {
        "url": "",
        "site": "http://192.140.163.220:9986/svip/lib/apiurl.txt",
        "dataKey": "dyyztvapiappyyds",
        "dataIv": "dyyztvapiappyyds",
        "deviceId": "",
        "version": "",
        "ua": ""
      }
    },
    {
      "key": "漫国",
      "name": "🍼漫国丨动漫",
      "type": 3,
      "api": "csp_AppSy",
      "ext": {
        "site": "http://192.140.163.220:9986/svip/lib/api.txt",
        "siteKey": "rectangleadsadxa",
        "listKey": "aassddwwxxllsx1x",
        "parsesKey": "aassddwwxxllsx1x"
      }
    },
    {
      "key": "樱花动漫",
      "name": "🍼樱花｜动漫",
      "type": 3,
      "api": "csp_XBPQ",
      "ext": "http://192.140.163.220:9986/svip/lib/樱花动漫.json"
    },
    {
      "key": "巴士动漫",
      "name": "🍼巴士｜动漫",
      "type": 3,
      "api": "csp_XYQHiker",
      "ext": "http://192.140.163.220:9986/svip/lib/巴士动漫.json"
    },
    {
      "key": "番薯",
      "name": "🍼番薯丨动漫",
      "type": 3,
      "quickSearch": 1,
      "api": "csp_AppGet",
      "ext": {
        "url": "https://new.app.bytegooty.com",
        "site": "",
        "dataKey": "N4yj7l7xKxHF4*gz",
        "dataIv": "N4yj7l7xKxHF4*gz",
        "deviceId": "",
        "version": "",
        "ua": ""
      }
    },
    {
      "key": "米饭",
      "name": "🍼米饭｜动漫",
      "type": 3,
      "quickSearch": 1,
      "api": "csp_AppGet",
      "ext": {
        "url": "https://get.mymifun.com",
        "site": "",
        "dataKey": "GETMIFUNGEIMIFUN",
        "dataIv": "GETMIFUNGEIMIFUN",
        "deviceId": "",
        "version": ""
      }
    },
    {
      "key": "比赛看球",
      "name": "🏀比赛｜看球",
      "type": 3,
      "api": "csp_Kanqiu",
      "gridview": 3,
      "style": {
        "type": "list"
      }
    },
    {
      "key": "梨园",
      "name": "🎭｜戏曲｜梨园",
      "jar": "http://192.140.163.220:9986/svip/lib/0711.gif;md5;05d526179fb3b9f8bb597e632b3a2273",
      "type": 3,
      "api": "http://192.140.163.220:9986/svip/lib/drpy2.min.js",
      "searchable": 1,
      "quickSearch": 1,
      "filterable": 1,
      "ext": "https://image-c.weimobwmc.com/ol-OlSf/983ec95b8248461cad620063864b21cd.jpg"
    },
    {
      "key": "本地",
      "name": "📻本地┃视频",
      "type": 3,
      "api": "csp_LocalFile"
    },
    {
      "key": "push_agent",
      "name": "✈Tg频道┃@nangemod",
      "type": 3,
      "api": "csp_Push",
      "searchable": 0,
      "ext": ""
    }
  ],
  "rules": [
    {
      "name": "头条",
      "hosts": [
        "toutiaovod.com"
      ],
      "regex": [
        "video/tos/cn"
      ]
    },
    {
      "name": "火山",
      "hosts": [
        "huoshan.com"
      ],
      "regex": [
        "item_id="
      ]
    },
    {
      "name": "抖音",
      "hosts": [
        "douyin.com"
      ],
      "regex": [
        "is_play_url="
      ]
    },
    {
      "name": "饭团点击",
      "hosts": [
        "dadagui",
        "freeok",
        "dadagui"
      ],
      "script": [
        "document.querySelector(\"#playleft iframe\").contentWindow.document.querySelector(\"#start\").click();"
      ]
    },
    {
      "name": "毛驴点击",
      "hosts": [
        "www.maolvys.com"
      ],
      "script": [
        "document.getElementsByClassName('swal-button swal-button--confirm')[0].click()"
      ]
    },
    {
      "name": "ofiii",
      "hosts": [
        "www.ofiii.com"
      ],
      "script": [
        "const play=document.getElementsByClassName('play_icon')[0],event=new MouseEvent('click',{bubbles:!0,cancelable:!0,view:window,screenX:100,screenY:100,clientX:50,clientY:50,button:0,shiftKey:!1,ctrlKey:!1,altKey:!1,metaKey:!1,modifierState:0});play.dispatchEvent(event);"
      ]
    }
  ],
  "doh": [],
  "hosts": [
    "cache.ott.*.itv.cmvideo.cn=base-v4-free-mghy.e.cdn.chinamobile.com",
    "cache.ott.ystenlive.itv.cmvideo.cn=base-v4-free-mghy.e.cdn.chinamobile.com",
    "cache.ott.bestlive.itv.cmvideo.cn=base-v4-free-mghy.e.cdn.chinamobile.com",
    "cache.ott.wasulive.itv.cmvideo.cn=base-v4-free-mghy.e.cdn.chinamobile.com",
    "cache.ott.fifalive.itv.cmvideo.cn=base-v4-free-mghy.e.cdn.chinamobile.com",
    "cache.ott.hnbblive.itv.cmvideo.cn=base-v4-free-mghy.e.cdn.chinamobile.com"
  ],
  "flags": [
    "youku",
    "优酷",
    "优 酷",
    "优酷视频",
    "qq",
    "腾讯",
    "腾 讯",
    "腾讯视频",
    "iqiyi",
    "qiyi",
    "奇艺",
    "爱奇艺",
    "爱 奇 艺",
    "m1905",
    "xigua",
    "letv",
    "leshi",
    "乐视",
    "乐 视",
    "sohu",
    "搜狐",
    "搜 狐",
    "搜狐视频",
    "tudou",
    "pptv",
    "mgtv",
    "芒果",
    "imgo",
    "芒果TV",
    "芒 果 T V",
    "bilibili",
    "哔 哩",
    "哔 哩 哔 哩"
  ],
  "ijk": [
    {
      "group": "软解码",
      "options": [
        {
          "category": 4,
          "name": "opensles",
          "value": "0"
        },
        {
          "category": 4,
          "name": "overlay-format",
          "value": "842225234"
        },
        {
          "category": 4,
          "name": "framedrop",
          "value": "1"
        },
        {
          "category": 4,
          "name": "soundtouch",
          "value": "1"
        },
        {
          "category": 4,
          "name": "start-on-prepared",
          "value": "1"
        },
        {
          "category": 1,
          "name": "http-detect-range-support",
          "value": "0"
        },
        {
          "category": 1,
          "name": "fflags",
          "value": "fastseek"
        },
        {
          "category": 2,
          "name": "skip_loop_filter",
          "value": "48"
        },
        {
          "category": 4,
          "name": "reconnect",
          "value": "1"
        },
        {
          "category": 4,
          "name": "enable-accurate-seek",
          "value": "0"
        },
        {
          "category": 4,
          "name": "mediacodec",
          "value": "0"
        },
        {
          "category": 4,
          "name": "mediacodec-auto-rotate",
          "value": "0"
        },
        {
          "category": 4,
          "name": "mediacodec-handle-resolution-change",
          "value": "0"
        },
        {
          "category": 4,
          "name": "mediacodec-hevc",
          "value": "0"
        },
        {
          "category": 1,
          "name": "dns_cache_timeout",
          "value": "600000000"
        }
      ]
    },
    {
      "group": "硬解码",
      "options": [
        {
          "category": 4,
          "name": "opensles",
          "value": "0"
        },
        {
          "category": 4,
          "name": "overlay-format",
          "value": "842225234"
        },
        {
          "category": 4,
          "name": "framedrop",
          "value": "1"
        },
        {
          "category": 4,
          "name": "soundtouch",
          "value": "1"
        },
        {
          "category": 4,
          "name": "start-on-prepared",
          "value": "1"
        },
        {
          "category": 1,
          "name": "http-detect-range-support",
          "value": "0"
        },
        {
          "category": 1,
          "name": "fflags",
          "value": "fastseek"
        },
        {
          "category": 2,
          "name": "skip_loop_filter",
          "value": "48"
        },
        {
          "category": 4,
          "name": "reconnect",
          "value": "1"
        },
        {
          "category": 4,
          "name": "enable-accurate-seek",
          "value": "0"
        },
        {
          "category": 4,
          "name": "mediacodec",
          "value": "1"
        },
        {
          "category": 4,
          "name": "mediacodec-auto-rotate",
          "value": "1"
        },
        {
          "category": 4,
          "name": "mediacodec-handle-resolution-change",
          "value": "1"
        },
        {
          "category": 4,
          "name": "mediacodec-hevc",
          "value": "1"
        },
        {
          "category": 1,
          "name": "dns_cache_timeout",
          "value": "600000000"
        }
      ]
    }
  ],
  "ads": [
    "static-mozai.4gtv.tv"
  ],
  "logo": "https://img.imgdd.com/506a1ca4-ab22-4c6e-b5aa-8033b95d2181.jpg ",
  "lives": [
    {
      "name": "live",
      "type": 0,
      "playerType": 1,
      "url": "http://192.140.163.220:9986/屿风眠星辞雾听澜书禾念安知夏遇秋寻冬观月.txt",
      "ua": "okhttp/3.15",
      "epg": "http://epg.51zmt.top:8000/e.xml",
      "logo": "https://img.imgdd.com/506a1ca4-ab22-4c6e-b5aa-8033b95d2181.jpg"
    }
  ]
}
```
### 备份[https://16409.kstore.vip/tv/beiyong.json]
```
{
  "spider": "https://oss4liview.moji.com/thd_file/2025/12/09/5280541da1ade1690d8d99efce29cb3c.jpg;md5;dca1d3d747bfd41f8124f789acfae162",
  "wallpaper": "https://cn.bing.com/th?id=OHR.RomeView_ZH-CN5882212305_1920x1080.jpg",
  "lives": [
    {
      "group": "redirect",
      "channels": [
        {
          "name": "live",
          "urls": [
            "proxy://do=live&type=txt&ext="
          ]
        }
      ]
    }
  ],
  "sites": [
    {
      "key": "豆豆",
      "name": "☯️微信公众号：北方学习天地(关注不迷路)",
      "type": 3,
      "api": "csp_DouDouGuard",
      "indexs": 1,
      "searchable": 0,
      "quickSearch": 0,
      "changeable": 0
    },
    {
      "key": "MDrive",
      "name": "🗂个人云盘┃配置",
      "type": 3,
      "api": "csp_MyDriveGuard",
      "changeable": 0,
      "indexs": 0,
      "searchable": 1,
      "style": {
        "type": "oval"
      },
      "ext": {
        "Cloud-drive": "tvfan/Cloud-drive.txt"
      }
    },
    {
      "key": "qiao4k1",
      "name": "☯️4K在线┃NGZ多源",
      "type": 3,
      "api": "csp_qiao2",
      "playerType": 2,
      "jar": "https://16409.kstore.vip/tv/flower.jar",
      "ext": "7lj763gg402i7942412h849jg1558j9jl8g19213il86316k465719lik158491h179312h4934183ig2l05g9"
    },
    {
      "key": "qiao4k2",
      "name": "☯️4K在线┃NGZ至臻",
      "type": 3,
      "api": "csp_Qiji",
      "playerType": 2,
      "jar": "https://16409.kstore.vip/tv/flower.jar",
      "ext": "7lj763gg402i7942553k9hj9lk5h8h86k4gg845kili63835585155k7k103470h1k955lhk9706ih80280ikij0g7l91jlhl565775ikjhj9lg422962ig23ili4845k6g492l0"
    },
    {
      "key": "csp_FeiMaoUC",
      "name": "⚡4K┃优汐UC",
      "type": 3,
      "api": "csp_Duopan",
      "filterable": 1,
      "jar": "https://img2.gelonghui.com/library/6b83e-77b913e7-5f64-4862-9a15-6222f3fc77c3.png;md5;6b83e9a49c3ef7cba2be8f11790b9236",
      "ext": {
        "site_urls": [
          "http://sduc.site/"
        ],
        "url_key": "FeiMaoUC"
      }
    },
    {
      "key": "csp_Netfixtv",
      "name": "⚡4k┃至臻",
      "type": 3,
      "api": "csp_Duopan",
      "filterable": 1,
      "jar": "https://img2.gelonghui.com/library/6b83e-77b913e7-5f64-4862-9a15-6222f3fc77c3.png;md5;6b83e9a49c3ef7cba2be8f11790b9236",
      "ext": {
        "site_urls": [
          "http://xiaomi666.fun",
          "https://xiaomiai.site",
          "https://mihdr.top",
          "https://www.mihdr.top",
          "http://www.miqk.cc",
          "https://www.zhizhenpan.fun"
        ],
        "url_key": "Netfixtv2",
        "token": "",
        "ucCookie": "",
        "quarkCookie": "",
        "threadinfo": {
          "chunksize": 450,
          "threads": 10
        }
      }
    },
    {
      "key": "csp_Duopan",
      "name": "⚡4K┃蜡笔",
      "type": 3,
      "api": "csp_Duopan",
      "filterable": 1,
      "jar": "https://img2.gelonghui.com/library/6b83e-77b913e7-5f64-4862-9a15-6222f3fc77c3.png;md5;6b83e9a49c3ef7cba2be8f11790b9236",
      "ext": {
        "site_urls": [
          "https://feimao666.fun",
          "http://feimao888.fun",
          "http://feimaoai.site",
          "http://www.labi88.sbs",
          "http://fmao.site",
          "http://fmao.shop",
          "http://xiaocge.fun"
        ],
        "url_key": "Duopan2"
      }
    },
    {
      "key": "新6V",
      "name": "🧲新6V┃磁力",
      "type": 3,
      "api": "csp_SixVGuard",
      "timeout": 10,
      "searchable": 1,
      "quickSearch": 1,
      "changeable": 0,
      "ext": "https://www.xb6v.com/"
    },
    {
      "key": "立播",
      "name": "🌟立播┃秒播",
      "type": 3,
      "api": "csp_LibvioGuard",
      "timeout": 10,
      "searchable": 1,
      "quickSearch": 1,
      "changeable": 1,
      "ext": {
        "Cloud-drive": "tvfan/Cloud-drive.txt",
        "from": "4k|auto"
      }
    },
    {
      "key": "厂长",
      "name": "📔厂长┃秒播",
      "type": 3,
      "api": "csp_NewCzGuard",
      "timeout": 10,
      "playerType": 2,
      "searchable": 1,
      "quickSearch": 1,
      "changeable": 1
    },
    {
      "key": "苹果",
      "name": "🍎苹果┃秒播",
      "type": 3,
      "api": "csp_LiteAppleGuard",
      "timeout": 10,
      "searchable": 1,
      "quickSearch": 1,
      "changeable": 1
    },
    {
      "key": "文采",
      "name": "💮文采┃秒播",
      "type": 3,
      "api": "csp_JpysGuard",
      "timeout": 10,
      "playerType": 2,
      "searchable": 1,
      "quickSearch": 1,
      "changeable": 1
    },
    {
      "key": "原创",
      "name": "☀原创┃秒播",
      "type": 3,
      "api": "csp_YCyzGuard",
      "timeout": 15,
      "playerType": 1,
      "searchable": 1,
      "quickSearch": 1,
      "changeable": 1
    },
    {
      "key": "比特",
      "name": "🍄比特┃秒播",
      "type": 3,
      "api": "csp_BttwooGuard",
      "timeout": 10,
      "searchable": 1,
      "quickSearch": 1,
      "changeable": 1
    },
    {
      "key": "热播",
      "name": "📺热播┃多线",
      "type": 3,
      "api": "csp_AppTTGuard",
      "timeout": 10,
      "playerType": 2,
      "searchable": 1,
      "quickSearch": 1,
      "changeable": 1,
      "ext": "uqGL1bNENExT7/hGxpSE5qU="
    },
    {
      "key": "剧圈",
      "name": "🐻剧圈┃多线",
      "type": 3,
      "api": "csp_JqqGuard",
      "timeout": 10,
      "searchable": 1,
      "quickSearch": 1,
      "changeable": 1
    },
    {
      "key": "下饭",
      "name": "🍙下饭┃多线",
      "type": 3,
      "api": "csp_AppSxGuard",
      "timeout": 10,
      "searchable": 1,
      "quickSearch": 1,
      "changeable": 1,
      "ext": "rfOX1voDIQhH8epBwpmIsuStr2HSytK11WwwcM/+XB8SbkBB4XYk/L52LroCiYRhoDrRLJ4ZkCh5q2RxCb5yUs6B5aCQVGSkR6zDgKLrFSDulg=="
    },
    {
      "key": "鲸鱼",
      "name": "👀鲸鱼┃多线",
      "type": 3,
      "api": "csp_AppSxGuard",
      "timeout": 10,
      "searchable": 1,
      "quickSearch": 1,
      "changeable": 1,
      "ext": "rfOX1voDIQhH8epBwtCFsra0umqGnpe+lWNtKs6yVR1PMxpf9CY77fshTPNP+c5lnl6CWfxm7XYO7C1p"
    },
    {
      "key": "橘子",
      "name": "🍊橘子┃多线",
      "type": 3,
      "api": "csp_AppSxGuard",
      "timeout": 10,
      "searchable": 1,
      "quickSearch": 1,
      "changeable": 1,
      "ext": "rfOb1uAWbkRHp7hdxprG9un3+TDC2t/rlTwlcMr+ChdbeV8Q9y9xsKxqfbtO0M05tGWcacFVm2c45jhyH6t1Rt6A6PjICGqxV+uN1uOqS2/x0Vp5J0Vfo8usQADpHg=="
    },
    {
      "key": "糯米",
      "name": "🍓糯米┃多线",
      "type": 3,
      "api": "csp_NmyswvGuard",
      "timeout": 15,
      "searchable": 1,
      "quickSearch": 1,
      "changeable": 1
    },
    {
      "key": "奥特",
      "name": "🏝奥特┃多线",
      "type": 3,
      "api": "csp_AueteGuard",
      "timeout": 10,
      "searchable": 1,
      "quickSearch": 1,
      "changeable": 1
    },
    {
      "key": "荐片",
      "name": "🐭荐片┃P2P",
      "type": 3,
      "api": "csp_JPJGuard",
      "timeout": 10,
      "playerType": 2,
      "searchable": 1,
      "quickSearch": 1,
      "changeable": 0
    },
    {
      "key": "Dm84",
      "name": "🚌巴士┃动漫",
      "type": 3,
      "api": "csp_Dm84Guard",
      "timeout": 10,
      "searchable": 1,
      "quickSearch": 1,
      "changeable": 1
    },
    {
      "key": "Anime1",
      "name": "🐾日本┃动漫",
      "type": 3,
      "api": "csp_Anime1Guard",
      "timeout": 10,
      "searchable": 1,
      "quickSearch": 1,
      "changeable": 1
    },
    {
      "key": "超全",
      "name": "⚽超全┃看球",
      "type": 3,
      "api": "csp_ZbzGuard",
      "searchable": 0,
      "quickSearch": 0,
      "changeable": 0,
      "style": {
        "type": "list"
      },
      "ext": "uqGL1fpJNAUf8fdTwZCE5qSp+Q=="
    },
    {
      "key": "88",
      "name": "⚽88┃看球",
      "type": 3,
      "api": "csp_Sir88Guard",
      "timeout": 10,
      "searchable": 0,
      "changeable": 0,
      "style": {
        "type": "list"
      }
    },
    {
      "key": "看球",
      "name": "⚽手机┃看球",
      "type": 3,
      "api": "csp_KanqiuGuard",
      "timeout": 10,
      "searchable": 0,
      "changeable": 0,
      "style": {
        "type": "list"
      }
    },
    {
      "key": "MTV1",
      "name": "🎙️易听┃音乐",
      "type": 3,
      "api": "csp_MusicGuard",
      "style": {
        "type": "rect",
        "ratio": 1
      },
      "searchable": 1,
      "quickSearch": 0,
      "changeable": 0
    },
    {
      "key": "MTV",
      "name": "🎶明星┃MV",
      "type": 3,
      "api": "csp_BiliGuard",
      "style": {
        "type": "rect",
        "ratio": 1.597
      },
      "searchable": 0,
      "quickSearch": 0,
      "changeable": 0,
      "ext": {
        "json": "https://nos.netease.com/ysf/5af5fbe12a88b7c45aa1c21e6551826c.txt"
      }
    },
    {
      "key": "有声小说",
      "name": "🎧有声┃小说",
      "type": 3,
      "api": "csp_Tingshu275Guard",
      "style": {
        "type": "rect",
        "ratio": 1
      },
      "searchable": 0,
      "quickSearch": 0,
      "changeable": 0
    },
    {
      "key": "Aid",
      "name": "🚑急救┃教学",
      "type": 3,
      "api": "csp_FirstAidGuard",
      "searchable": 0,
      "quickSearch": 0,
      "changeable": 0,
      "style": {
        "type": "rect",
        "ratio": 3.8
      }
    },
    {
      "key": "抠搜",
      "name": "🍄抠抠┃搜搜",
      "type": 3,
      "api": "csp_KkSsGuard",
      "searchable": 1,
      "quickSearch": 1,
      "changeable": 0,
      "ext": {
        "Cloud-drive": "tvfan/Cloud-drive.txt",
        "from": "4k|auto"
      }
    },
    {
      "key": "UC",
      "name": "🌈优汐┃搜搜",
      "type": 3,
      "api": "csp_UuSsGuard",
      "searchable": 1,
      "quickSearch": 1,
      "changeable": 0,
      "ext": {
        "Cloud-drive": "tvfan/Cloud-drive.txt",
        "from": "4k|auto"
      }
    },
    {
      "key": "YiSo",
      "name": "😹易搜┃三盘",
      "type": 3,
      "api": "csp_YiSoGuard",
      "searchable": 1,
      "quickSearch": 1,
      "changeable": 0,
      "ext": {
        "Cloud-drive": "tvfan/Cloud-drive.txt",
        "from": "4k|auto",
        "yiSoCookie": "satoken=4437cb8c-a260-411b-9a0d-1fa622ab422f"
      }
    },
    {
      "key": "米搜",
      "name": "🦋米搜┃优夸",
      "type": 3,
      "api": "csp_MIPanSoGuard",
      "searchable": 1,
      "quickSearch": 1,
      "changeable": 0,
      "ext": {
        "Cloud-drive": "tvfan/Cloud-drive.txt",
        "from": "4k|auto"
      }
    },
    {
      "key": "xzso",
      "name": "👻盘他┃夸父",
      "type": 3,
      "api": "csp_XzsoGuard",
      "searchable": 1,
      "quickSearch": 1,
      "changeable": 0,
      "ext": {
        "Cloud-drive": "tvfan/Cloud-drive.txt",
        "from": "4k|auto"
      }
    },
    {
      "key": "YpanSo",
      "name": "🐟盘她┃夸父",
      "type": 3,
      "api": "csp_YpanSoGuard",
      "searchable": 1,
      "quickSearch": 1,
      "changeable": 0,
      "ext": {
        "Cloud-drive": "tvfan/Cloud-drive.txt",
        "from": "4k|auto"
      }
    },
    {
      "key": "push_agent",
      "name": "🛴手机┃推送",
      "type": 3,
      "api": "csp_PushGuard",
      "searchable": 0,
      "quickSearch": 0,
      "ext": {
        "Cloud-drive": "tvfan/Cloud-drive.txt",
        "from": "4k|auto"
      }
    },
    {
      "key": "Bili",
      "name": "🅱哔哔合集┃弹幕",
      "type": 3,
      "api": "csp_BiliGuard",
      "style": {
        "type": "rect",
        "ratio": 1.597
      },
      "searchable": 1,
      "quickSearch": 0,
      "changeable": 0,
      "ext": {
        "json": "https://nos.netease.com/ysf/0075389dca9afadd4614e9713765ff17.txt"
      }
    },
    {
      "key": "Biliych",
      "name": "🅱哔哔演唱会┃弹幕",
      "type": 3,
      "api": "csp_BiliGuard",
      "style": {
        "type": "rect",
        "ratio": 1.597
      },
      "searchable": 1,
      "quickSearch": 0,
      "changeable": 0,
      "ext": {
        "json": "https://nos.netease.com/ysf/6496356286589c68f52c2f99c0c674c7.txt"
      }
    },
    {
      "key": "dr_兔小贝",
      "name": "📚儿童┃启蒙",
      "type": 3,
      "api": "https://git.yylx.win/https://raw.githubusercontent.com/fantaiying7/EXT/refs/heads/main/drpy2.min.js",
      "ext": "https://git.yylx.win/https://raw.githubusercontent.com/fantaiying7/EXT/refs/heads/main/兔小贝.js",
      "style": {
        "type": "rect",
        "ratio": 1.597
      },
      "searchable": 0,
      "quickSearch": 0,
      "changeable": 0
    },
    {
      "key": "少儿教育",
      "name": "📚少儿┃教育",
      "type": 3,
      "api": "csp_BiliGuard",
      "style": {
        "type": "rect",
        "ratio": 1.597
      },
      "searchable": 0,
      "quickSearch": 0,
      "changeable": 0,
      "ext": {
        "json": "https://nos.netease.com/ysf/89370c8ddf36b5e1beb4d71adb921bda.txt"
      }
    },
    {
      "key": "小学课堂",
      "name": "📚小学┃课堂",
      "type": 3,
      "api": "csp_BiliGuard",
      "style": {
        "type": "rect",
        "ratio": 1.597
      },
      "searchable": 0,
      "quickSearch": 0,
      "changeable": 0,
      "ext": {
        "json": "https://nos.netease.com/ysf/d7a21cf34ede56f5c686ecfba5fc7e3f.txt"
      }
    },
    {
      "key": "初中课堂",
      "name": "📚初中┃课堂",
      "type": 3,
      "api": "csp_BiliGuard",
      "style": {
        "type": "rect",
        "ratio": 1.597
      },
      "searchable": 0,
      "quickSearch": 0,
      "changeable": 0,
      "ext": {
        "json": "https://nos.netease.com/ysf/8f55d520f8d70056695740ef151744a7.txt"
      }
    },
    {
      "key": "高中教育",
      "name": "📚高中┃课堂",
      "type": 3,
      "api": "csp_BiliGuard",
      "style": {
        "type": "rect",
        "ratio": 1.597
      },
      "searchable": 0,
      "quickSearch": 0,
      "changeable": 0,
      "ext": {
        "json": "https://nos.netease.com/ysf/c66a4b5356141c49fd45ec51568017b4.txt"
      }
    },
    {
      "key": "fan",
      "name": "Tg频道@nangemod",
      "type": 3,
      "api": "csp_XPathGuard",
      "searchable": 1,
      "quickSearch": 0,
      "changeable": 0
    },
    {
      "key": "cc",
      "name": "请勿相信视频中广告",
      "type": 3,
      "api": "csp_XPathGuard",
      "searchable": 1,
      "quickSearch": 0,
      "changeable": 0
    }
  ],
  "parses": [
    {
      "name": "解析",
      "type": 3,
      "url": "Demo"
    },
    {
      "name": "解析二",
      "type": 2,
      "url": "Sequence"
    },
    {
      "name": "解析三",
      "type": 1,
      "url": "https://zy.qiaoji8.com/neibu.php?url=",
      "ext": {
        "flag": [
          "qq",
          "腾讯",
          "qiyi",
          "爱奇艺",
          "奇艺",
          "youku",
          "优酷",
          "sohu",
          "搜狐",
          "letv",
          "乐视",
          "mgtv",
          "芒果",
          "tnmb",
          "seven",
          "bilibili"
        ],
        "header": {
          "User-Agent": "okhttp/4.9.1"
        }
      }
    },
    {
      "name": "备用解析",
      "type": 1,
      "url": "https://zy.qiaoji8.com/gouzi.php?url=",
      "ext": {
        "flag": [
          "qq",
          "腾讯",
          "qiyi",
          "爱奇艺",
          "奇艺",
          "youku",
          "优酷",
          "sohu",
          "搜狐",
          "letv",
          "乐视",
          "mgtv",
          "芒果",
          "tnmb",
          "seven",
          "bilibili",
          "1905",
          "NetFilx"
        ],
        "header": {
          "User-Agent": "okhttp/4.9.1"
        }
      }
    },
    {
      "name": "备用解析二",
      "type": 1,
      "url": "https://zy.qiaoji8.com/xiafan.php?url=",
      "ext": {
        "flag": [
          "QD4K",
          "iyf",
          "duanju",
          "gzcj",
          "GTV",
          "GZYS",
          "weggz",
          "Ace"
        ],
        "header": {
          "User-Agent": "okhttp/4.9.1"
        }
      }
    }
  ],
  "flags": [
    "youku",
    "qq",
    "QQ",
    "iqiyi",
    "qiyi",
    "letv",
    "sohu",
    "pptv",
    "PPTV",
    "mgtv",
    "wasu",
    "bilibili",
    "m1905",
    "seven",
    "m78",
    "mtv",
    "sjs",
    "dbs",
    "yds",
    "HNB",
    "JL4K"
  ],
  "ijk": [
    {
      "group": "软解码",
      "options": [
        {
          "category": 4,
          "name": "opensles",
          "value": "0"
        },
        {
          "category": 4,
          "name": "overlay-format",
          "value": "842225234"
        },
        {
          "category": 4,
          "name": "framedrop",
          "value": "1"
        },
        {
          "category": 4,
          "name": "soundtouch",
          "value": "1"
        },
        {
          "category": 4,
          "name": "start-on-prepared",
          "value": "1"
        },
        {
          "category": 1,
          "name": "http-detect-range-support",
          "value": "0"
        },
        {
          "category": 1,
          "name": "fflags",
          "value": "fastseek"
        },
        {
          "category": 2,
          "name": "skip_loop_filter",
          "value": "48"
        },
        {
          "category": 4,
          "name": "reconnect",
          "value": "1"
        },
        {
          "category": 4,
          "name": "enable-accurate-seek",
          "value": "0"
        },
        {
          "category": 4,
          "name": "mediacodec",
          "value": "0"
        },
        {
          "category": 4,
          "name": "mediacodec-auto-rotate",
          "value": "0"
        },
        {
          "category": 4,
          "name": "mediacodec-handle-resolution-change",
          "value": "0"
        },
        {
          "category": 4,
          "name": "mediacodec-hevc",
          "value": "0"
        },
        {
          "category": 1,
          "name": "dns_cache_timeout",
          "value": "600000000"
        }
      ]
    },
    {
      "group": "硬解码",
      "options": [
        {
          "category": 4,
          "name": "opensles",
          "value": "0"
        },
        {
          "category": 4,
          "name": "overlay-format",
          "value": "842225234"
        },
        {
          "category": 4,
          "name": "framedrop",
          "value": "1"
        },
        {
          "category": 4,
          "name": "soundtouch",
          "value": "1"
        },
        {
          "category": 4,
          "name": "start-on-prepared",
          "value": "1"
        },
        {
          "category": 1,
          "name": "http-detect-range-support",
          "value": "0"
        },
        {
          "category": 1,
          "name": "fflags",
          "value": "fastseek"
        },
        {
          "category": 2,
          "name": "skip_loop_filter",
          "value": "48"
        },
        {
          "category": 4,
          "name": "reconnect",
          "value": "1"
        },
        {
          "category": 4,
          "name": "enable-accurate-seek",
          "value": "0"
        },
        {
          "category": 4,
          "name": "mediacodec",
          "value": "1"
        },
        {
          "category": 4,
          "name": "mediacodec-auto-rotate",
          "value": "1"
        },
        {
          "category": 4,
          "name": "mediacodec-handle-resolution-change",
          "value": "1"
        },
        {
          "category": 4,
          "name": "mediacodec-hevc",
          "value": "1"
        },
        {
          "category": 1,
          "name": "dns_cache_timeout",
          "value": "600000000"
        }
      ]
    }
  ],
  "ads": [
    "43.248.128.138"
  ],
  "logo": "https://img.imgdd.com/506a1ca4-ab22-4c6e-b5aa-8033b95d2181.jpg"
}
```