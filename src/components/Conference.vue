<template>
    <div class="">
      <div id="videos">
        <div id="publisher"></div>
        <div id="subscriber"></div>
      </div>
    </div>
</template>

<script src="https://static.opentok.com/v2/js/opentok.min.js"></script>
<script>
import axios from 'axios';
import OT from '@opentok/client';

export default {
  name: 'Conference',
  methods: {
    // Handling all of our errors here by alerting them
    handleError(error) {
        if (error) {
            console.log(error.message);
        }
    },
    //Get schedule from server
    async getConference() {  
      try {
        // Call api
        const { data: conference } = await axios.get('http://localhost/conference');
        //console.log(conference);
        return conference;
      // Error
      } catch (error) {
        console.error(error);
      }
    },
    // Show other people
    showRemotePublisher(session, remotePublisher) {
        // subscribe to remote publisher
        let subscribedPublisher = session.subscribe (
            remotePublisher.stream, // publisher - event.stream
            'subscriber', // Id of DOM element
            {
                insertMode: 'append', // options
                width: '12vw',
                height: '12vw',
                style: { nameDisplayMode: "on" }
            },
            this.handleError, // error
        );

        return subscribedPublisher;
    },
    // Function to start video call
         startVideoCall(apiKey, sessionId, token) {

            // Initialise session
            var session = OT.initSession(apiKey, sessionId);

            // Store publishers
            var allStreams = [];
            var allSubscribedPublishers = [];

            // Subscribe to a newly created stream
            session.on('streamCreated', function(remotePublisher) {

                // Get remote publisher
                //console.log(`remotePublisher : ${JSON.stringify(remotePublisher)}`);

                // Store in stream list
                allStreams.push(remotePublisher);
                console.log(`allStreams count : ${allStreams.length}`);

                // If less than certain amount of publishers, 
                if (allStreams.length <= 2) {

                    // Subscribe to remote publisher straight away
                    let subscribedPublisher = this.showRemotePublisher(session, remotePublisher);
                    //console.log(`subscribedPublisher : ${JSON.stringify(subscribedPublisher)}`);

                    // Store in subscribed publishers array
                    allSubscribedPublishers.push(subscribedPublisher);
                    console.log(`allSubscribedPublishers : ${allSubscribedPublishers.length}`);
                }
            });

            // Rotate publishers displayed on screen
            function rotateRemotePublishers() {

                // If we have more than the max displayable publishers
                if (allStreams.length > 2) {
                    console.log(`rotating`);

                    // Get subscriber to be removed from display / remove from allSubscribedPublishers list
                    let unsubscribedPublisher = allSubscribedPublishers.shift();

                    // Usubscribe from first subscribed publisher
                    session.unsubscribe(unsubscribedPublisher);

                    // Get random remote publisher
                    let remotePublisher = allStreams[Math.floor(Math.random() * allStreams.length)]

                    // Subscribe to remote publisher
                    let subscribedPublisher = showRemotePublisher(session, remotePublisher);

                    // Store in subscribed publishers array
                    allSubscribedPublishers.push(subscribedPublisher);

                } else {
                    console.log(`not rotating`);
                }
            }
            // Roate every 10 seconds
            setInterval(rotateRemotePublishers, 10000);

            // Create a publisher
            var publisher = OT.initPublisher(
                'publisher', // Id of DOM element
                {
                    insertMode: 'append',
                    width: '12vw',
                    height: '12vw',
                    name: Math.floor(Math.random() * 10),
                    style: { nameDisplayMode: "on" }
                },
                this.handleError,
            );

            // Connect to the session
            session.connect(token, function(error) {
                // If the connection is successful, publish to the session
                if (error) {
                    this.handleError(error);
                } else {
                    session.publish(publisher, this.handleError);
                }
            });
        },

  },
  mounted() {
    //Get schedule from server
    this.getConference()
    .then(
      (conference) => {
        console.log(conference);
        var apiKey = conference.apiKey;
        var sessionId = conference.sessionId;
        var token = conference.token;
        
        this.startVideoCall(apiKey, sessionId, token);
      }
    );
  }
};
</script>

<style scoped lang="scss">

</style>
