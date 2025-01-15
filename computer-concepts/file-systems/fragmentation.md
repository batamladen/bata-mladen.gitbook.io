# Fragmentation

Fragmentation in computing refers to the inefficient use of storage space, which occurs when files are divided into pieces and scattered across a disk or memory.

For example, lets say you downloaded 3 movies, red, blue and green, below is how they will be stored on the hard drive.

<figure><img src="../../.gitbook/assets/3 movies on disk.png" alt="" width="563"><figcaption></figcaption></figure>

Now lets say you didn't like the second movie and decide to delete it

<figure><img src="../../.gitbook/assets/2 movies on a disk.png" alt="" width="554"><figcaption></figcaption></figure>

Now this movies is not deleted as you would imagine, but the computer now knows that it can overwrite this space with other data, and that's why it is counting it in free space, although the movie is still there. And if this space is not used, you can get the data before, that is - the second movie. This is how data retrival is done.

But lets say now you saw a 3h Stiven Spilsberg movie and want to download it. This is how the disk would look now.

<figure><img src="../../.gitbook/assets/long movie ond diks.png" alt="" width="553"><figcaption></figcaption></figure>

The device wants to utalise the "free" space and splits the movie in parts. This process is called **fragmentation**

When the movie is loaded into the RAM it works fine. But since it is on different places on the disk it takes a bit more time to load it to the RAM. and now imagine this done on almost all the files. Your computer slows down and you start to think maybe its because of the "Maria sent you a message" ads you clicked. But in fact your data is fragmented all over the place on the disk... and you downloaded a malware from the ad but nvm that now.
