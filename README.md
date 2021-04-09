<p align="center">
  <a href="https://dev.to/vumdao">
    <img alt="HAProxy Basic Login Authentication" src="https://github.com/vumdao/haproxy-basic-auth/blob/master/cover.jpg?raw=true" width="500" />
  </a>
</p>
<h1 align="center">
  <div><b>HAProxy Basic Login Authentication</b></div>
</h1>

### - Here we use http-request <action> [options...] [ { if | unless } <condition> ]

### - The http-request statement defines a set of rules which apply to layer 7 processing. The rules are evaluated in their declaration order when they are met in a frontend, listen or backend section. Any rule may optionally be followed by an ACL-based condition, in which case it will only be evaluated if the condition is true.

### - There is no limit to the number of http-request statements per instance.

---

## 🚀 **Setup HAProxy config which contains basic login to access the dashboard and allow access for special resource IP**
- How to generate haproxy encrypted password
```
printf "thepassword" | mkpasswd --stdin --method=sha-256
```

- Modify [haproxy.cfg](https://github.com/vumdao/haproxy-basic-auth/blob/master/haproxy.cfg) which allow access for requests from source `18.69.61.21` but requires login for others

```
userlist AuthUsers
        user haproxyreport password $5$3VeorK1XxvgRseQ$VBkOPCY2enWZsas.C6X9Iif0FPHDknXXXXXXXXX

frontend fe-verify
        bind *:443 ssl crt /etc/certs

        acl haproxy_report hdr(host) haproxy-report.cloudopz.co

        http-request set-header X-Forwarded-Proto https if { ssl_fc }
        use_backend haproxy-report-backend if haproxy_report

# haproxy-report-backend
backend haproxy-report-backend
        acl authorized http_auth(AuthUsers)
        acl nagios src 18.69.61.21
        http-request allow if nagios
        http-request auth realm haproxyreport if !authorized
        server haproxy-report 127.0.0.1:1800
```

**More about haproxy**
- [How To Set HTTP-Request Header In Haproxy](https://dev.to/vumdao/how-to-set-http-request-header-in-haproxy-48bd)
- [How To Block IP Addresses In HAProxy](https://dev.to/vumdao/how-to-block-ip-addresses-in-haproxy-3f84)
- [HAProxy With Resolvers In Case Of AWS Application LoadBalancer](https://dev.to/vumdao/haproxy-with-resolvers-in-case-of-aws-application-loadbalancer-d1n)
- [Use GoAccess To Analyze HAProxy Logs](https://dev.to/vumdao/use-goaccess-to-analyze-haproxy-logs-2m51)

[Reference](https://www.haproxy.org/download/2.4/doc/configuration.txt)

<h3 align="center">
  <a href="https://dev.to/vumdao">:stars: Blog</a>
  <span> · </span>
  <a href="https://github.com/vumdao/">Github</a>
  <span> · </span>
  <a href="https://vumdao.hashnode.dev/">Web</a>
  <span> · </span>
  <a href="https://www.linkedin.com/in/vu-dao-9280ab43/">Linkedin</a>
  <span> · </span>
  <a href="https://www.linkedin.com/groups/12488649/">Group</a>
  <span> · </span>
  <a href="https://www.facebook.com/CloudOpz-104917804863956">Page</a>
  <span> · </span>
  <a href="https://twitter.com/VuDao81124667">Twitter :stars:</a>
</h3>
