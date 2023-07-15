# cloudflare 

## how to redirect openai

使用场景：重定向 openai by [cloudflare](https://www.geeksforgeeks.org/what-is-cloudflare/)，目的是无障碍访问。 
把弄好的url添加到苹果快捷指令，Siri可以调用快捷指令，从而通过Siri喊话chatgpt

苹果快捷指令: https://www.icloud.com/shortcuts/f54d104f83a9413385e68b8f5a55ac38

openai重定向代码
```
// Website you intended to retrieve for users.
const upstream = 'api.openai.com'

// Custom pathname for the upstream website.
const upstream_path = '/'

// Website you intended to retrieve for users using mobile devices.
const upstream_mobile = upstream

// Countries and regions where you wish to suspend your service.
const blocked_region = []

// IP addresses which you wish to block from using your service.
const blocked_ip_address = ['0.0.0.0', '127.0.0.1']

// Whether to use HTTPS protocol for upstream address.
const https = true

// Whether to disable cache.
const disable_cache = false

// Replace texts.
const replace_dict = {
    '$upstream': '$custom_domain',
}
addEventListener('fetch', event => {
    event.respondWith(fetchAndApply(event.request));
})

async function fetchAndApply(request) {

    let response = null;
    let method = request.method;


    let url = new URL(request.url);
    let url_hostname = url.hostname;
    url.protocol = 'https:';
    url.host = 'api.openai.com';


    let request_headers = request.headers;
    let new_request_headers = new Headers(request_headers);
    new_request_headers.set('Host', url.host);
    new_request_headers.set('Referer', url.protocol + '//' + url_hostname);

   

    let original_response = await fetch(url.href, {
        method: method,
        headers: new_request_headers,
        body: request.body
    })
  

    let original_response_clone = original_response.clone();
    let original_text = null;
    let response_headers = original_response.headers;
    let new_response_headers = new Headers(response_headers);
    let status = original_response.status;

    new_response_headers.set('Cache-Control', 'no-store');
    new_response_headers.set('access-control-allow-origin', '*');
    new_response_headers.set('access-control-allow-credentials', true);
    new_response_headers.delete('content-security-policy');
    new_response_headers.delete('content-security-policy-report-only');
    new_response_headers.delete('clear-site-data');

    original_text = original_response_clone.body
    response = new Response(original_text, {
        status,
        headers: new_response_headers
    })

    return response

}

async function replace_response_text(response, upstream_domain, host_name) {
    let text = await response.text()

    var i, j;
    for (i in replace_dict) {
        j = replace_dict[i]
        if (i == '$upstream') {
            i = upstream_domain
        } else if (i == '$custom_domain') {
            i = host_name
        }

        if (j == '$upstream') {
            j = upstream_domain
        } else if (j == '$custom_domain') {
            j = host_name
        }

        let re = new RegExp(i, 'g')
        text = text.replace(re, j);
    }
    return text;
}


async function device_status(user_agent_info) {
    var agents = ["Android", "iPhone", "SymbianOS", "Windows Phone", "iPad", "iPod"];
    var flag = true;
    for (var v = 0; v < agents.length; v++) {
        if (user_agent_info.indexOf(agents[v]) > 0) {
            flag = false;
            break;
        }
    }
    return flag;
}
```
