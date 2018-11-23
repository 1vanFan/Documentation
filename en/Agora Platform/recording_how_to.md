
---
title: Recording-related Issues
description: 
platform: Recording-related Issues
updatedAt: Fri Nov 23 2018 06:21:55 GMT+0000 (UTC)
---
# Recording-related Issues
### How do you check the recording permissions?

* iOS/macOS: The first system launch will prompt you to set the recording permissions. If you manually close the settings, you can query the system interface by calling AVAudioSession.sharedInstance().recordPermission()[iOS8.0+] to check the recording permissions.

* Android: Different manufacturers implement different methods to check the recording permissions. The Agora Native SDK can detect a recording failure and report an ERR_ADM_RECORD_AUDIO_FAILED (1018) error code, but this method may not cover all phones. Your app can prompt the user to check the recording permissions by reporting the recording error code.

### Can I record in an audio or video call?

Yes, Agora provides recording API methods and allows you to store recorded files on specified servers.

### Should I use the Agora Recording Server, Agora Recording SDK, or CDN Recording?

The Agora Recording SDK is the recommended solution for all users.

### Do I need to configure the ARS IP in the backend Dashboard?

No.

### Do I need a recording key to record?

No.

### Is the recording trigger controlled by the server?

Yes.

### Do I need to call the startRecordingServer method to trigger the recording?

No.

### How does the server know when to join a channel and the channel details since the recording is triggered by the server?

The server decides when to join a channel (start recording) or leave a channel (stop recording) through your signaling layer. Joining a channel with the Agora Recording SDK is similar joining a channel with the Agora Native SDK at the client side.

### How do I know when the recording is complete?
Automatic recording mode: If no one is in the channel when the idle time is up, the Agora Recording SDK stops recording and leaves the channel. The application monitoring the leaveChannel callback indicates that the recording is complete and moves to the next step, such as upload the recording file to another server.

### How do I customize the recording file and set the directory of the recording file?

To customize the recording file, you need to configure the CFG file. The SDK package has a hidden .cfg.json file under Agora_Recording_SDK_for_Linux_FULL/samples/cpp. You can refer to the method of setting the output file path and filename. See [Recording API](../../en/Recording/recording_cpp.md).

For example, the recording file needs to be saved as "root directory/dates/uid.aac" where /home/Agora_Recording_SDK_for_Linux_FULL/samples/recording_output is the root directory.

### Can I record the voice or video of a specific user?
This function is not supported. You will record the audio or video of all users.

