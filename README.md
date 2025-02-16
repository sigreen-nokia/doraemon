                                          
                     ,#(((##((((((((((&       
                  %((((#     &&   %((((((#    
                #&(%, &   @@ ,      (((((((#  
               &&   ,  *  @@   . &  ((((((((( 
               #,%.#    *% #%,#   #    ((((((@
              ..,&#.           ....,,,,, &(((#
               .         #       &(       (((@
                  %     ,        ,*   /   #(( 
                       &/         /(     &(#  
                #%%                     #(    
               %&(  @%&,             .(%      
        %   %   %   @*/(%(&%%%%%%%%%@((       
              ((@,   #%%      &((((((((#      
               ,(&,           &(((%(((((.     
                ((((&        ((((((%((((,#    
         &     %(((((          (((%,*         
          ,,,,,,,,,,,*         #(,  ,,//,     
                      (       %,              
                        ,*/*,     


## What is doraemon:

* A docker that receives Defender webhooks and notifies a Line(app) account of DDoS events

## Licence

* doraemon is Opensource. 
* doraemon uses gazoo as a base, gazoo is also opensource. https://github.com/sigreen-nokia/gazoo 

* The topology could not be much simpler
   
           ############      ############            ######################
           #          #      #          #            #                    #
           # Network  #      # Deefield #            # osx/linux/win      #
           # Under    # ---> # Defender # -webhook-> #                    # -Line message API->
           # Attack   #      #          #            # running doraemon   #
           #          #      #          #            # docker             #
           #          #      #          #            #                    #
           ############      ############            ######################
     
## platforms

* should run on pretty much any docker or docker desktop
* I've tested this on my mac running docker for desktop (for ARM CPU's enable rosseta in advanced settings)
* I've also tested this on Ubuntu Linux 20.04. Install docker using your favorite site 
* I've also tested this on Windows 10 using wsl and docker for desktop 

## pre requisits

* Docker 

## Steps to configure your line account for message api (if you have not already)

* create an official line account
* on the line management console https://manager.line.biz
*    create a channel
* on the line developer console https://developers.line.biz
*   in the channel enable the message api and add a long lived access token.
*   then within the channel the grab the following info to use below:
*   User ID
*   Channel access token (long-lived)

## configure doraemon to use your line account

* Set these two lines in file scripts/default.sh to the 'user id' and 'channel access token (long lived)' for your line account. 

```
export LINE_USER_ID="U720xxxxxxxxxxxxxxcf"
export LINE_CHANNEL_ACCESS_TOKEN_LL="14Id1pxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxDnyilFU="
```

## run doraemon as follows 

```
cd  doraemon #(you must be in this doraemon  git when you run docker)
docker run -d  -v /tmp/gazoo-commands:/tmp/gazoo-commands --restart always --name=gazoo -v ${PWD}/scripts:/scripts -p 8080:8080 simonjohngreen/gazoo
```

## Steps to configure the webhook on Deepfield
* The assumption is that you have port 8080 opened up all the way to the docker
*  In the defender ui
*  Admin->notification->[add]
*  name doraemon        
*  tick webhook         
*  url: http://[your docker hosts ip or fqdn]:8080
*  click test, you should see a green test sent successfully if your firewall routers and docker allow the 8080 port traffic
*  save
*  add this notification as the action in your Defender policies

## Then what ?

*       start or wait for a DDoS event 
*       you should see DDoS related messages on your line account

## Developer ? 
* If you are a developer and would preffer to build your own container using my source code see https://github.com/sigreen-nokia/gazoo 


