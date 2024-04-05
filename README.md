# HyChain Node GCP Runner

Setup your Hychain node in the cloud for minimal cost.

Topia donations appreciated if you feel so inclined:  
0xC18Aa275a85fb351342cDde4b0e66FceFd0ABA18


1.  Go to https://console.cloud.google.com  
    1.1 Make sure you are logged in to your account ( create an account if needed).
    1.2 In the top left click `Select a project`, create a new project if you don't already have one.
    <img width="1433" alt="image" src="https://github.com/trevordowdle/hychain-node-runner-gcp/assets/4210581/51f54dc2-f02d-4d8a-9df3-6737874f27ec">
    1.3 Once created select you project from the drop down.
    <img width="1105" alt="image" src="https://github.com/trevordowdle/hychain-node-runner-gcp/assets/4210581/ad30e7c8-918c-4c19-8bee-52bd4197194e">
    1.4 In the search box, type and navigate to Cloud Functions
    <img width="1433" alt="image" src="https://github.com/trevordowdle/hychain-node-runner-gcp/assets/4210581/9eb492aa-3e53-45f4-8283-97aacb8e7dac">
    1.5 You will be prompted to sign up for a free trial or enable billing before you can create a cloud function.  Do so.  Note: The cost to run a node is minimal, and will likely fall within the free tier of good cloud function usage.  I will update the readme with more information on costs once I have more data.
2.  With billing or your free trial enabled, click "Create Function"
    2.0 Note: as you are creating your funtion Google may prompt you to enable certain services, enable everything as required.  
    2.1 Name your function whatever you'd like `node-runner`  
    2.2 Pick your region to run the function out of `us-central1` is fine.  
    2.3 Make sure Trigger Https, and Require Authentication are selected.  
    2.4 Click to toggle or open the `Runtime, build, connections and security settings`
        2.4.1 Set the memory allocated to 4GB, CPU to 2 and Timeout to 900 (can be adjusted higher if needed but even 600 should suffice for most use cases), Concurrency to 1, autoscaling min to 0 and max to 1.  The Default compute service account should be selected as well.
  <img width="636" alt="image" src="https://github.com/trevordowdle/hychain-node-runner-gcp/assets/4210581/b4de5b36-e174-48f7-850b-a2dd82f7adf3">
  2.5 Click next to get to the Code section, from here make sure Node.js 20 is selected in the runtime dropdown and change the source code dropdown to zip upload.
  2.6 Set the entry point to `runGuardianNode`
  2.7 Select a destination bucket (doesn't matter where), create a destination bucket if you don't already have one.
  2.8 Upload zip file - The zip file you need to upload is called `function-source.zip` and is included with this github repo you can click into it and download it directly from github.
  <img width="1057" alt="image" src="https://github.com/trevordowdle/hychain-node-runner-gcp/assets/4210581/fcb773b0-b714-4c9a-980c-3a119e3fe3db">




  

