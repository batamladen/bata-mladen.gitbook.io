# OSINT/Waldo's Night Out

{% hint style="info" %}
**Description:**

After a long night exploring the Northern Virginia area, Waldo woke up with a horrible headache and realized he forgot the events of the previous night. Can you help him remember where he was?



Enter the flag as three towns/cities that each collection of photos was taken in, separated by underscores, removing all special characters and spaces (e.g. pctf{NewYorkCity\_Philadelphia\_NewOrleans}).
{% endhint %}



We are provided with the following zip file that contains 3-4 images from each location.

{% file src="../../.gitbook/assets/images.zip" %}

### Location 1

Looks like an abandoned tramboline park, we know that waldo was in North Virginia so we google:\
`` `Abandoned tramboline park north virginia` `` go to videos and we see a youtuber exploring the tramboline park with a description of the location.

Location: Sterling



### Location 2

Location 2 are three images taken from a top of an building with a massive highway and big glass buildings, google the highway in North Virginia  and follow through what cities it goes, write them down to chatgt and send the clearest image we have. We get the answer.

Location: Reston



### Location 3

Looks like an abandoned old lux hotel. Only one ussefull image in here from the interior and we also have this image:



<figure><img src="../../.gitbook/assets/IMG_7499.jpg" alt=""><figcaption></figcaption></figure>

it has a paper that says, THIS AREA IN NOT OPEN FOR LIQUIDATION and a says ICL/INTERNATIONAL CONTENT LIQUIDATIONS INC.

ICL is a liquidation company specializing in hotel/resort liquidations.



We open their website and see they have a "Past Sales" section

<figure><img src="../../.gitbook/assets/image (35).png" alt=""><figcaption></figcaption></figure>



Search for Virginia. We see 3 hotels, and looking at one of the images we are provided with we see the similarity with one of the listed hotels.

<figure><img src="../../.gitbook/assets/IMG_7471.jpg" alt=""><figcaption></figcaption></figure>



<figure><img src="../../.gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>



Location: Tysons



### Flag

pctf{Sterling\_Reston\_Tysons}
