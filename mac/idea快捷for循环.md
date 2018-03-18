使用Intellij idea 时，想要快捷生成for循环代码块


itar 生成array for代码块

    for (int i = 0; i < array.length; i++) {  
                 = array[i];  
                  
            }  

itco 生成Collection迭代 

    for (Iterator<String> iterator = locationUrl.iterator(); iterator.hasNext(); ) {  
                String next =  iterator.next();     
            }              
            
iten 生成enumeration遍历

    while (enumeration.hasMoreElements()) {  
                Object nextElement =  enumeration.nextElement();  
            }  


iter 生成增强forxun

[java] view plain copy

    for (String s : locationUrl) {  
                  
            }  

itit  生成iterator 迭代

    while (iterator.hasNext()) {  
                    Object next =  iterator.next();                        
     }  


itli 生成List的遍历

    for (int i = 0; i < locationUrl.size(); i++) {  
                String s =  locationUrl.get(i);                    
            }    
            
ittok 生成String token遍历

    for (StringTokenizer stringTokenizer = new StringTokenizer(TVSOU_URL); stringTokenizer.hasMoreTokens(); ) {  
                String s = stringTokenizer.nextToken();                    
           }  


itve 生成Vector数组迭代

    for (int i = 0; i < vector.size(); i++) {  
                Object elementAt =  vector.elementAt(i);                    
            }  

另外两个和循环无关，只是方便创建 

itaws 生成Axis2 web service调用

    try {  
                MyServiceStub stub = new MyServiceStub();  
                stub.sayHelloWorldFrom();  
            } catch (Exception ex) {  
                ex.printStackTrace();  
            }  

itws 生成 Axis web service调用

    try {  
                MyServiceLocator locator = new MyServiceLocator();  
                Activator service = locator.get();  
                // If authorization is required  
                //((MyService_Soap_BindingStub)service).setUsername("user3");  
                //((MyService_Soap_BindingStub)service).setPassword("pass3");  
                // invoke business method  
                service.businessMethod();  
            } catch (javax.xml.rpc.ServiceException ex) {  
                ex.printStackTrace();  
            } catch (java.rmi.RemoteException ex) {  
                ex.printStackTrace();  
            }  
            