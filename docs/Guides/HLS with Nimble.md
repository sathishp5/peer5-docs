# Nimble how-to

## Intro to nimble
WMSPanel's Nimble is a fantastic lightweight media server
We are going to show how to configure Nimble and stream HLS with Peer5.


### Setup CORS
Peer5 needs CORS to get the video segments from 
Open your config file. (Usually located at "/etc/nimble/nimble.conf")
Add the following lines at the bottom of the file:
access_control_allow_origin = *
access_control_allow_headers = Range
access_control_expose_headers = Content-Length, Content-Range
Save the file
Restart the nimble service using this command: "sudo service nimble restart"

Setup HLS Streams (from WMSPanel):

VOD
Copy the mp4 video file you want to stream to a path that will be accessible by the nimble user on your server, Lets say /var/www/video/.
On the navigation panel: Nimble streamer -> Edit nimble routes






Select	ADD VOD STREAMING ROUTE on the top right. 
Fill the form and press OK.


	
your route will be added to the table, select GET URL for player, copy the url and replace sample.mp4 with your video file name. You might also need to replace the IP address with your server’s external IP.
you can now play the url in your HLS player.

		












Live (RTMP transmuxing to HLS)

On the navigation panel: Nimble streamer -> Live streams settings




Under Global tab -  Set chunk duration to 3 and Chunk count to 10. Then press Save.



Select Live pull settings tab.



Select Add RTMP URL
Fill your RTMP stream url, application name (i.e live) and name for your ouput stream. Then press Save.








Go back to the top navigation bar and select Live streams.




Choose your server from the list on the bottom.



Click Outgoing stream (the pink part of the drawing), your stream should appear.
Press the question mark on the right side of your stream’s line.
This window will appear:



You might need to replace the IP address with the external address of your server.
Your HLS stream can now be played via this url.
