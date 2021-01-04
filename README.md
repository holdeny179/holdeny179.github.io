{
    "outbounds": [{
        "protocol": "vmess",
        "settings": {
            "vnext": [{
                "address": "162.222.180.58",
                "port": 51714,
                "users": [{
                    "id": "7e944552-63ff-d9b7-35dc-196f0cf0a615",
                    "alterId": 0,
                    "security": "auto"
                }]
            }]
        },
        "streamSettings": {
            "network": "tcp",
            "tcpSettings": {
                "header": {
                    "type": "http",
                    "request": {
                        "version": "1.1",
                        "method": "GET",
                        "path": ["/"],
                        "headers": {
                            "Host": ["www.cloudflare.com", "www.amazon.com"],
                            "User-Agent": ["Mozilla/5.0(WindowsNT10.0;WOW64)AppleWebKit/537.36(KHTML,likeGecko)Chrome/55.0.2883.75Safari/537.36", "Mozilla/5.0(WindowsNT6.3;Win64;x64)AppleWebKit/537.36(KHTML,likeGecko)Chrome/61.0.3163.100Safari/537.36", "Mozilla/5.0(WindowsNT10.0;Win64;x64)AppleWebKit/537.36(KHTML,likeGecko)Chrome/62.0.3202.75Safari/537.36", "Mozilla/5.0(WindowsNT10.0;Win64;x64;rv:57.0)Gecko/20100101Firefox/57.0"],
                            "Accept": ["text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8"],
                            "Accept-language": ["zh-CN,zh;q=0.8,en-US;q=0.6,en;q=0.4"],
                            "Accept-Encoding": ["gzip,deflate,br"],
                            "Cache-Control": ["no-cache"],
                            "Pragma": "no-cache"
                        }
                    }
                }
            }
        },
        "mux": {
            "enabled": true
        }
    }, {
        "protocol": "freedom",
        "settings": {},
        "tag": "direct"
    }, {
        "protocol": "blackhole",
        "settings": {},
        "tag": "blocked"
    }, {
        "protocol": "dns",
        "tag": "dns-out"
    }],
    "inbounds": [{
        "port": "4100",
        "protocol": "dokodemo-door",
        "settings": {
            "network": "tcp,udp",
            "timeout": 0,
            "followRedirect": true
        },
        "sniffing": {
            "enabled": true,
            "destOverride": ["http", "tls"]
        }
    }, {
        "port": 2133,
        "tag": "dns-in",
        "protocol": "dokodemo-door",
        "settings": {
            "address": "119.29.29.29",
            "port": 53,
            "timeout": 0,
            "network": "tcp,udp"
        }
    }, {
        "port": 2333,
        "protocol": "socks",
        "settings": {
            "auth": "noauth",
            "udp": true
        }
    }, {
        "port": 6666,
        "protocol": "http",
        "settings": {
            "auth": "noauth",
            "udp": true
        }
    }],
    "dns": {
        "servers": [{
            "address": "119.29.29.29",
            "port": 53,
            "domains": ["geosite:cn"],
            "expectIPs": ["geoip:cn"]
        }, {
            "address": "1.1.1.1",
            "port": 53,
            "domains": ["geosite:geolocation-!cn"]
        }, "8.8.8.8", "localhost"]
    },
    "routing": {
        "domainStrategy": "IPOnDemand",
        "rules": [{
            "type": "field",
            "inboundTag": ["dns-in"],
            "outboundTag": "dns-out"
        }, {
            "type": "field",
            "ip": ["geoip:private"],
            "outboundTag": "blocked"
        }, {
            "type": "field",
            "ip": ["geoip:cn"],
            "outboundTag": "direct"
        }, {
            "type": "field",
            "domain": ["geosite:cn"],
            "outboundTag": "direct"
        }]
    }
}
