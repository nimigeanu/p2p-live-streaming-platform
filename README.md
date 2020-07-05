# P2P Assisted Live Streamig Platform

## Highlights

* Connects video players into a peer-to-peer network
* Whenever possible, download video content from peers rather than CDN
* Reduces CDN traffic consumption
* Improves overall video start time and quality, reduces buffering
* Runs in AWS
* Deploys in minutes


### Deploying the platform

1. Sign in to the [AWS Management Console](https://aws.amazon.com/console), then click the button below to launch the CloudFormation template. Alternatively you can [download](template.yaml) the template and adjust it to your needs.

[![Launch Stack](https://cdn.jsdelivr.net/gh/buildkite/cloudformation-launch-stack-button-svg@master/launch-stack.svg)](https://console.aws.amazon.com/cloudformation/home#/stacks/create/review?stackName=p2p-live-streaming&templateURL=https://lostshadow.s3.amazonaws.com/p2p-streaming-platform/template.yaml)

2. Choose your streaming server `InstanceType`:
	* `t3.nano` is the cheapest, and suitable for most scenarios
	* `t3.micro` is similarly appropriate if you're taking advantage of the [AWS Free Tier](https://aws.amazon.com/free/), otherwise slightly more expensive than the nano
3. Hit the `Create Stack` button. 
4. Wait for the `Status` to become `CREATE_COMPLETE`. Note that this may take up to **5 minutes** or more.
5. Under `Outputs`, notice the keys named `IngressEndpoint` and `TestPlayerUrl`; write these down for using later

### Testing your setup

1. Point your RTMP broadcaster (any of [these](https://support.google.com/youtube/answer/2907883) will work) to the rtmp `IngressEndpoint` output by CloudFormation above

	Note that, while some RTMP broadcasters require a simple URI, others (like [OBS Studio](https://obsproject.com)) require a **Server** and **Stream key**. In this case, split the URI above at the last *slash* character, as following:
	
	**Server**: `rtmp://[HOST]/live`  
	**Stream key**: `stream001`

	Also note that **the enpoint may not be ready** as soon as CloudFormation stack is complete; it may take a couple minutes more for the server software to be compiled, installed and started on the virtual server so be patient

2. Test the video playback by opening the above `TestPlayerUrl` (as output by CloudFormation) in a browser
	
	Once again, node that this may not be available right away; resources do take time to get set up, especially on lower-end virtual server instances

3. Duplicate the above video playback window in various browsers/devices; notice the `Connected peers`, `Data downloaded from peers` and `Data uploaded to peers` information fields to assess the efficiency of your peering setup

### Integration, customization, notes

* Proof of concept has been reduced to the most basic functionality so as to be easily understood and integrated
* P2P capability is based on the amazing resources [here](https://github.com/novage/p2p-media-loader)
* Simplified setup makes use of public STUN servers and WebTorrent trackers; setting up your own should be considered for production
* The sample uses [Plyr](https://plyr.io) as its test player; many others are supported