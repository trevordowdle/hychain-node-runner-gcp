# HyChain Node GCP Runner

Setup your Hychain node in the cloud using GCP for minimal cost.

Topia donations appreciated if you feel so inclined: 0xC18Aa275a85fb351342cDde4b0e66FceFd0ABA18


1.  Go to https://console.cloud.google.com  
    1.1. Make sure you are logged in to your account ( create an account if needed).  
    1.2. In the top left click `Select a project`, create a new project if you don't already have one.  

    <img width="1433" alt="image" src="https://github.com/trevordowdle/hychain-node-runner-gcp/assets/4210581/51f54dc2-f02d-4d8a-9df3-6737874f27ec">  

    1.3. Once created select your project from the drop down.
    
    <img width="1105" alt="image" src="https://github.com/trevordowdle/hychain-node-runner-gcp/assets/4210581/ad30e7c8-918c-4c19-8bee-52bd4197194e"> 
    
    1.4. In the search box, type and navigate to Cloud Functions
    
    <img width="1433" alt="image" src="https://github.com/trevordowdle/hychain-node-runner-gcp/assets/4210581/9eb492aa-3e53-45f4-8283-97aacb8e7dac"> 
    
    1.5. You will be prompted to sign up for a free trial or enable billing before you can create a cloud function.  Do so.  Note: The cost to run a node is minimal, and will likely fall within the free tier of google cloud function usage.  (I will update the readme with more information on costs once I have more data.)
    
2.  With billing or your free trial enabled, click "Create Function"  
    2.0. Note: as you are creating your funtion Google may prompt you to enable certain services, enable everything as required.    
    2.1. Name your function whatever you'd like `node-runner`  
    2.2. Pick your region to run the function out of `us-central1` is fine.  
    2.3. Make sure Trigger Https, and Require Authentication are selected.  
    2.4. Click to toggle or open the `Runtime, build, connections and security settings`  
    2.5. Set the memory allocated to 4GB, CPU to 2 and Timeout to 900 (can be adjusted higher if needed but even 600 should suffice for most use cases), Concurrency to 1, autoscaling min to 0 and max to 1.  The Default compute service account should be selected as well.
    
    <img width="636" alt="image" src="https://github.com/trevordowdle/hychain-node-runner-gcp/assets/4210581/b4de5b36-e174-48f7-850b-a2dd82f7adf3">
    
    2.6. Click next to get to the Code section, from here make sure Node.js 20 is selected in the runtime dropdown and change the source code dropdown to zip upload.  
    2.7. Set the entry point to `runGuardianNode`  
    2.8. Select a destination bucket (doesn't matter where), create a destination bucket if you don't already have one.  
    2.9. Upload zip file - The zip file you need to upload is called `function-source-run-node.zip` and is included with this github repo you can click into it and download it directly from github.
    
    <img width="1057" alt="image" src="https://github.com/trevordowdle/hychain-node-runner-gcp/assets/4210581/fcb773b0-b714-4c9a-980c-3a119e3fe3db">
    
    2.10. At the bottom left click deploy  
    2.11. Wait for your function to deploy successfully  
    2.12. Now we'll want to edit the function to include your `private-key` in the code.  This is the private key that holds your nodes or as reccomended the private key of the wallet you have `delegated` to.  Click `Edit`, then along the top click `Code`.  It will open up with a `index.js` file, on line 8 paste your private key to replace the existing text within the quotes. (note your function source is secure)
    
    2.12.a. Note: the source code has since been updated, in the next line below you will enter your public wallet address, it will need to be the public wallet that actually owns your node keys.  This public address is used to log how many rewards you have earned each time your function is called.  (It will show up in your function logs so you can stay up to date on how many rewards you've earned)

    <img width="690" alt="image" src="https://github.com/trevordowdle/hychain-node-runner-gcp/assets/4210581/d70446c6-f133-4184-97fc-7ebbdff96f43">

    
    2.13. `Deploy` your function again with the updated change, and wait for your function to deploy sucessfully.  
    2.14. Make note of your function URL, click the copy icon to copy the url to your clipboard.  
    
    <img width="953" alt="image" src="https://github.com/trevordowdle/hychain-node-runner-gcp/assets/4210581/1e2b0768-a8a7-4c1f-8a43-c219d10be39e">

    
4.  Type and go into Cloud Scheduler  

    <img width="749" alt="image" src="https://github.com/trevordowdle/hychain-node-runner-gcp/assets/4210581/9856d30d-5643-4cab-b456-847a8a4c3cb5">

    3.1.  Click `Create Job`  
    3.2.  Enter a name for your job  
    3.3.  Under frequency put `0 */6 * * *` this will run the node every 6 hours.  
    3.4.  Set it to run based on a timezone of your choosing.  

    <img width="594" alt="image" src="https://github.com/trevordowdle/hychain-node-runner-gcp/assets/4210581/77230ecd-9d3a-4abd-a1ba-39930cea4f35">
    
    3.5. Click `Configure the execution`  
    3.6. Select `Http` as the target type.  
    3.7. Paste in the URL you copied in step `2.12.`, if you need to get the url again navigate to https://console.cloud.google.com/functions, click into your function from the list and from there you should be able to copy your function url.  
    3.8.  Http method should be post.  
    3.9.  Auth Header Add OIDC token
    3.10. Under Service Account select `App Engine default service account`

    <img width="594" alt="image" src="https://github.com/trevordowdle/hychain-node-runner-gcp/assets/4210581/7cb83cc8-7d4c-4175-9716-5a067c32f342">  

    3.11.  Click create at the bottom  
    3.12.  Once that is created you should be set, you can select your cloud scheduled job from the list and click the `Force Run` option to run it right away, otherwise it will keep running at it's scheduled times (every 6 hours).  
   

### Helper URLs to manage your deployment:  
1.  https://console.cloud.google.com/functions
    Manage your function from here, view logs to confirm it's running successfully etc..
2.  https://console.cloud.google.com/cloudscheduler
    Manage your scheduler from here, you can disable it temporarily or update how often it runs etc..

Feel free to reach out to me in Discord `Nofacetimber` for additional help, or to discuss unique usecases, if you have a lot of keys it may make sense to tweak these settings such as as timeout length and how frequently it runs.  


Thanks to `niftyorca` for the inspiration with his awesome githubs actions solution.


### Note for the claim rewards function you will follow the same steps above with the following changes: 

    2.7. Set the entry point to `claimNodeRewards`  
    2.9. Upload zip file - The zip file you need to upload is called `function-source-claim-rewards.zip` and is included with this github repo you can click into it and download it directly from github.
    3.3.  Your choice here, you can keep frequency blank and just trigger the Cloud Scheduler manually (using "Force Run") every time you'd like to claim, or say you want to claim once a month, you can put the frequency at `0 0 1 * *` to run it at the beginning of each month. 




  

