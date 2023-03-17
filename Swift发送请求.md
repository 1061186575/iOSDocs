

# 使用Swift发送请求

```swift
import Foundation


func request(url: String) {
    let url = URL(string: url)!
    
    let task = URLSession.shared.dataTask(with: url) {(data, response, error) in
        guard let data = data else { return }
        print("\n\n")
        print(String(data: data, encoding: .utf8)!)
    }
    
    task.resume()
}
request(url: "https://zhoup.top/")



func request2(url: String) {
    let url = URL(string: url)!
    var request = URLRequest(url: url)
    request.httpMethod = "GET"

    NSURLConnection.sendAsynchronousRequest(request, queue: OperationQueue.main) {(response, data, error) in
        guard let data = data else { return }
        print("\n\n")
        print(String(data: data, encoding: .utf8)!)
    }
}
request2(url: "https://baidu.com/")
```


# 发送JSON请求
```swift

let url = URL(string: "http://localhost:3010/ios/test")!

func get() {
    let session = URLSession.shared
    session.dataTask(with: url) { (data, response, error) in
        if let response = response {
            print(response)
        }
        
        if let data = data {
            print("data", data)
            do {
                let json = try JSONSerialization.jsonObject(with: data, options: [])
                print("json", json)
            } catch {
                print(error)
            }
            
        }
    }.resume()
}


func post() {
    let parameters = ["username": "admin", "motto": "HelloWorld"]
    
    
    var request = URLRequest(url: url)
            request.httpMethod = "POST"
            request.addValue("application/json", forHTTPHeaderField: "Content-Type")
            guard let httpBody = try? JSONSerialization.data(withJSONObject: parameters, options: []) else { return }
            request.httpBody = httpBody
            
            let session = URLSession.shared
            session.dataTask(with: request) { (data, response, error) in
                if let response = response {
                    print(response)
                }
                
                if let data = data {
                    do {
                        let json = try JSONSerialization.jsonObject(with: data, options: [])
                        print(json)
                    } catch {
                        print(error)
                    }
                }
                
            }.resume()
}

get()
post()

```



# 请求忽略https证书错误
如果https的证书有问题, 请求会报错, 无法拿到数据, 使用下面这个方法发请求, 即使证书有问题也可以拿到数据

```swift

import Foundation


var url = "https://file.zhoup.top/FqqSaRPQ7W"


// 忽略https证书错误
class MyHttp: NSObject, URLSessionDelegate {
    
    func httpGet(request: URLRequest,  completionHandler: @escaping (Data?, URLResponse?, Error?) -> Void) {
        let configuration = URLSessionConfiguration.default
        let session = URLSession(configuration: configuration, delegate: self, delegateQueue:OperationQueue.main)
        let dataTask = session.dataTask(with: request, completionHandler: completionHandler)

        dataTask.resume()
    }
     
    func urlSession(_ session:URLSession, didReceive challenge:URLAuthenticationChallenge, completionHandler:@escaping (URLSession.AuthChallengeDisposition,URLCredential?) -> Void) {
 
            guard challenge.protectionSpace.authenticationMethod == "NSURLAuthenticationMethodServerTrust"else {
                return
            }
 
            let credential = URLCredential.init(trust: challenge.protectionSpace.serverTrust!)
            completionHandler(.useCredential,credential)
      }
}



let myhttp = MyHttp()
var request = URLRequest(url: URL(string: url)!)
request.httpMethod = "GET"

myhttp.httpGet(request: request, completionHandler: {(data, response, error) -> Void in
    if error != nil{
        print(error?.localizedDescription)
    }else{
        let str = String(data: data!, encoding: String.Encoding.utf8)
        print("访问成功，获取数据如下：")
        print(str!)
    }
})


// https://www.cnblogs.com/ziwuxian/p/16288673.html

```

