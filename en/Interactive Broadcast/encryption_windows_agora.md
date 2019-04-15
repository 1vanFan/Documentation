
---
title: Implement Encryption
description: 
platform: Windows
updatedAt: Tue Feb 19 2019 09:39:55 GMT+0800 (CST)
---
# Implement Encryption
This page introduces various encryption modes. Choose one that best suits your needs.

> Both Communication and Live Broadcast support encryption. For live broadcasts, if you need to use CDN for streaming, recording, and storage, do not use encryption.

## Scenario 1: Do Not Use Encryption

The [Agora SDK for Windows](https://docs.agora.io/en/Agora%20Platform/downloads) does not include any independent library for encryption. You are all set if you do not use encryption.

## Scenario 2: Use Encryption

The following figure shows how Agora’s communications use built-in encryption:

<img alt="../_images/agora-encryption_en.png" src="https://web-cdn.agora.io/docs-files/en/agora-encryption_en.png" />


### Step 1: Enable encryption.

Call the `setEncryptionSecret` method to enable built-in encryption and set the encryption password.

### Step 2: Choose the encryption mode to use.

Call the `setEncryptionMode` method to set the built-in encryption mode.

## Scenario 3: Use Customized Encryption

The following figure shows the data encryption/decryption process:

<img alt="../_images/developer-encryption_en.png" src="https://web-cdn.agora.io/docs-files/en/developer-encryption_en.png" />


## Step 1: Register a Packet Observer

The Agora Native SDK allows your application to register a packet observer to receive events whenever a voice or video packet is transmitting.

Register a packet observer on your application by using the following method:

```
virtual int registerPacketObserver(IPacketObserver* observer);
```

The observer must be inherited from <code>agora::IPacketObserver</code> and be implemented in C++. The following is the definition of the <code>IPacketObserver</code> class:

```
class IPacketObserver
{
public:

struct Packet
{
        /** Buffer address of the sent or received data.
         */
const unsigned char* buffer;
        /** Buffer size of the sent or received data.
         */
unsigned int size;
};
/** An audio packet is sent to other users.

     @param packet See Packet.
     @return
     - true: The packet is sent successfully.
     - false: The packet is discarded.
     */
virtual bool onSendAudioPacket(Packet& packet) = 0;
/** A video packet is sent to other users.

     @param packet See Packet.
     @return
     - true: The packet is sent successfully.
     - false: The packet is discarded.
     */
virtual bool onSendVideoPacket(Packet& packet) = 0;
/** An audio packet is sent by other users.

     @param packet See Packet.
     @return
     - true: The packet is received successfully.
     - false: The packet is discarded.
*/
virtual bool onReceiveAudioPacket(Packet& packet) = 0;
/** A video packet is sent by other users.

     @param packet See Packet.
     @return
     - true: The packet is received successfully.
     - false: The packet is discarded.
*/
virtual bool onReceiveVideoPacket(Packet& packet) = 0;
};
```

## Step 2: Implement a Customized Data Encryption Algorithm

The observer must be inherited from <code>agora::IPacketObserver</code> to be implemented in the customized data encryption algorithm on your application. The following example uses XOR for data processing. For the Agora Native SDK, sending and receiving packets are handled by different threads, which is why encryption and decryption can use different buffers:

```
class AgoraPacketObserver : public agora::IPacketObserver
 {
             public:
                 AgoraPacketObserver()
                 {
                     m_txAudioBuffer.resize(2048);
                     m_rxAudioBuffer.resize(2048);
                     m_txVideoBuffer.resize(2048);
                     m_rxVideoBuffer.resize(2048);
                 }
                 virtual bool onSendAudioPacket(Packet& packet)
                 {
                     int i;
                     //encrypt the packet
                     const unsigned char* p = packet.buffer;
                     const unsigned char* pe = packet.buffer+packet.size;


                              for (i = 0; p < pe && i < m_txAudioBuffer.size(); ++p, ++i)
                     {
                         m_txAudioBuffer[i] = *p ^ 0x55;
                     }
                     //assign new buffer and the length back to SDK
                     packet.buffer = &m_txAudioBuffer[0];
                     packet.size = i;
                     return true;
                 }

                 virtual bool onSendVideoPacket(Packet& packet)
                 {
                     int i;
                     //encrypt the packet
                     const unsigned char* p = packet.buffer;
                     const unsigned char* pe = packet.buffer+packet.size;
                     for (i = 0; p < pe && i < m_txVideoBuffer.size(); ++p, ++i)
                     {
                         m_txVideoBuffer[i] = *p ^ 0x55;
                     }
                     //assign the new buffer and the length back to the SDK
                     packet.buffer = &m_txVideoBuffer[0];
                     packet.size = i;
                     return true;
                 }

                 virtual bool onReceiveAudioPacket(Packet& packet)
                 {
                     int i = 0;
                     //decrypt the packet
                     const unsigned char* p = packet.buffer;
                     const unsigned char* pe = packet.buffer+packet.size;
                     for (i = 0; p < pe && i < m_rxAudioBuffer.size(); ++p, ++i)
                     {
                         m_rxAudioBuffer[i] = *p ^ 0x55;
                     }
                     //assign the new buffer and the length back to the SDK
                     packet.buffer = &m_rxAudioBuffer[0];
                     packet.size = i;
                     return true;
                 }

                 virtual bool onReceiveVideoPacket(Packet& packet)
                 {
                     int i = 0;
                     //decrypt the packet
                     const unsigned char* p = packet.buffer;
                     const unsigned char* pe = packet.buffer+packet.size;


                             for (i = 0; p < pe && i < m_rxVideoBuffer.size(); ++p, ++i)
                     {
                         m_rxVideoBuffer[i] = *p ^ 0x55;
                     }
                     //assign the new buffer and the length back to the SDK
                     packet.buffer = &m_rxVideoBuffer[0];
                     packet.size = i;
                     return true;
                 }

             private:
                 std::vector<unsigned char> m_txAudioBuffer; //buffer for sending the voice data
                 std::vector<unsigned char> m_txVideoBuffer; //buffer for sending the video data

                 std::vector<unsigned char> m_rxAudioBuffer; //buffer for receiving the voice data
                 std::vector<unsigned char> m_rxVideoBuffer; //buffer for receiving the video data
     };
```

## Step 3: Register the Instance

Call the <code>registerAgoraPacketObserver</code> method to register the instance of the <code>agora::IPacketObserver</code> class implemented by your application.



