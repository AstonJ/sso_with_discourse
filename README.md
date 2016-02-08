# sso_with_discourse
Implementation in ruby for https://meta.discourse.org/t/using-discourse-as-a-sso-provider/32974

Example
Using with Rails.

Step 1: create a "lib" directory in your app's root directory and copy the sso_with_discourse.rb into it.

Step 2: Add "lib" to autoload path by adding the following line to config/application.rb in your Application class defination.

    config.autoload_paths << "#{Rails.root}/lib"
    
Step 3: make an initializer in config/initializer, you can name it as sso_with_discourse.rb
create the setting for the class to read from as below.

     SsoWithDiscourse::Sso.config = {
 
                                 :sso_url=>"http://www.example.com/session/sso_provider", 
 
                                 :return_url=>"http://www.my-app.com/path_to_processing_action", 
 
                                 :sso_secret=>"yourreallyrandomsecurekey"
 
                                 } 
 

Now you can use this class in your controllers.
get the request url for redirecting the user to the discourse sso end point.

   sso = SsoWithDiscourse::Sso.new
   
   session[:sso] = sso
   
   redirect_to sso.request_url
  
in the action for processing the return request, you need to get the sso instance from the session because it is required that the nonce
returned from Discourse matches the one stored in the instance as part of the parsing.

   sso = session[:sso]
   
   sso.parse(params)
   
If the verification is successful, you can inspect the following result:

  sso.status  #supposed to be "success".
  
  sso.message #supposed to be "SSO verification passed."
  
  sso.user_info #the user information returned by Discourse.
  
  




