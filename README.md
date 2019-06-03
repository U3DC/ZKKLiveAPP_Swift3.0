# ZKKLiveAPP_Swift3.0
##### 项目变化简介:
- 使用swift3开发 与之前的swfit2相比有一些简化 具体参考
Swift3.0新特性：http://www.cocoachina.com/swift/20160712/17028.html
- 使用网络请求简单封装
````
	
	public class func get(_ url: String!, _ para: NSDictionary?, callBack: @escaping(_ data:Data? ,_ error:Error?) -> Void) -> Void{
		
		let  session = URLSession.shared
		
		let urlStr = NSMutableString.init(string: url)
		
		if let para = para {
			urlStr.append(encodeUniCode(parasToString(para) as NSString) as String)
		}
		
		var request = URLRequest(url: URL.init(string: urlStr as String)!)
		request.httpMethod = "GET"
		let dataTask = session.dataTask(with: request) { (data, responseObject, error) in

			guard let httpURLResponse = responseObject as? HTTPURLResponse, httpURLResponse.statusCode == 200,error == nil else{
				print("DataTask Error:\(error)")
				return
			}
			
			callBack(data, error);

		}
		
		dataTask.resume()
		
	}
	
	public class func post(_ url:String! , _ para:NSDictionary?, callBack: @escaping(_ data:Data? ,_ error : Error?) -> Void) -> Void{
	
		let  session = URLSession.shared;
		
		let urlStr = NSMutableString.init(string: url)
		if para != nil{
			
			urlStr.append(encodeUniCode(parasToString((para)!) as NSString) as String)

		}
		
		var request = URLRequest.init(url: URL.init(string: urlStr as String)!)
		request.httpMethod = "POST"
		let dataTask  = session.dataTask(with: request) { (data, responseObject, error) in
			
			guard let httpURLResponse = responseObject as? HTTPURLResponse, httpURLResponse.statusCode == 200, error == nil else{
			print("DataTask Error:\(error)")
			return
			}
			callBack(data,error)
		}
		
		dataTask.resume()
	
	}
	
	
	/* 特殊字符 */
	final class func encodeUniCode(_ string: NSString) ->NSString{
		
		return string.addingPercentEncoding(withAllowedCharacters: .urlFragmentAllowed)! as NSString
	}
	
	final class func parasToString (_ para:NSDictionary ) -> String {
		
		let paraString = NSMutableString.init(string: "?")
		for (key ,value) in para as! [String: String] {
			paraString.append("\(key)=\(value)&")
		}
		if paraString.hasSuffix("&"){
			paraString.deleteCharacters(in: NSMakeRange(paraString.length - 1, 1))
		}
		return String(paraString)
	}

````
- 图片加载
````
	
		/* 加载 */
		
		DispatchQueue.global().async { 
			
			let request = URLRequest(url: URL.init(string: urlString as String)!)
			
			URLSession.shared.dataTask(with: request) { (data, response, error) in
				guard
					let httpURLResponse = response as? HTTPURLResponse, httpURLResponse.statusCode == 200,
					let mimeType = response?.mimeType, mimeType.hasPrefix("image"),
					let data = data, error == nil,
					let image = UIImage(data: data)
					else { return }
				
				self.contentMode = UIViewContentMode.scaleToFill
				DispatchQueue.main.async(execute: {
					self.image = image
				})
				}.resume()

			
		}
		
		
		/* 缓存后续在添加，当然无法与SDImage性能相媲美，等待稳定版本合适的第三方框架再用吧 */
		

````
- Cell特效 
````
		cell.layer.transform = CATransform3DMakeScale(0.8, 0.8, 1)
		UIView.animate(withDuration: 0.8, animations: {
			cell.layer.transform = CATransform3DMakeScale(1, 1, 1)
		}, completion: nil)

````

** 功能详情 **
###### 简书：https://www.jianshu.com/p/ed9eb96afa78
