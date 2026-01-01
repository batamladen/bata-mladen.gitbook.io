# Email Investigation

## Understand Email Basics

An increasing number of enterprises are now using email as their primary communication mode. The growing dependence on emails has also given rise to email crimes. Therefore, forensic investigators need to have a complete understanding of an email system and its inner architecture, along with the components that work together to deliver an email from a sender to recipients.

***

Introduction to an Email System

Email is an abbreviation of “electronic mail,” which is used for sending, receiving, and saving messages over electronic communication systems. With growing reliance on technology, email has become one of the most popular modes of communication. An email system works on the basic client–server architecture. It allows clients to send/receive mails via email servers that communicate with one another. Most email systems have text editors with basic formatting options that enable clients to compose text messages and send them to one or more recipients. Once the message has been sent, it passes through several servers and is stored in the mailbox of the recipient until he/she retrieves it.

***

Components Involved in Email Communication

There are several components of email communication that play specific roles when an email message is transmitted from a sender to a recipient.

**Mail User Agent:** Also known as an email client, mail user agent (MUA) is a desktop application for reading, sending, and organizing emails. It provides an interface for users to receive, compose, or send emails from their configured email addresses. A user needs to set up and configure their email address before using the email client. The configuration includes issuing email IDs, passwords, Post Office Protocol version 3 (POP3)/ Internet Message Access Protocol (IMAP) and Simple Mail Transfer Protocol (SMTP) address, a port number, and other related preferences. There are many standalone and web-based email clients such as Claws Mail, Thunderbird, Mailbird, Zimbra Desktop, Gmail, and Outlook.com.

**Mail Transfer Agent:** Mail transfer agent (MTA) is an important component of the email transmission process. It is primarily a type of mail server that receives the email message from the mail submission agent and decrypts the header 4information to see where the message is going. Once determined, it passes on the message to the next MTA server. All the MTA servers talk to each other via the SMTP protocol. Some examples of MTA include Sendmail, Exim, and Postflix.

**Mail Delivery Agent:** Mail delivery agent (MDA) is the server that receives the email message from the last MTA and keeps it in the mailbox of the recipient. Dovecot is an example of an MDA.

***

### Email Servers

**SMTP Server:** The SMTP is an outgoing mail server that allows a user to send emails to a valid email address. Users cannot use the SMTP server to receive emails; however, in conjunction with the Post Office Protocol (POP) or IMAP, they can use the SMTP to receive emails with proper configuration. Any SMTP server is assigned an address by the mail client of the user in the following format: smtp.serveraddress.com (for example, SMTP server address of Gmail would be smtp.gmail.com). When a user sends an email to a specific recipient, it first reaches the SMTP server that processes the message to determine the address of the recipient and then relays it to the particular server. All SMTP servers generally listen to port 25. However, outgoing SMTP servers use port 587 for transport layer security connections and port 465 for secure sockets layer (SSL) connections.



**POP3 Server:** POP3 is a simple protocol for retrieving emails from an email server. When the POP server receives emails, they are stored on the server until and unless the user requests it. The POP3 server does not allow the concept of folders; it considers the mailbox on the server to be its sole store. Once the user connects to the mail server to recover their mail using the email client, the mails are automatically downloaded from the mail server to the user’s hard disk and are no longer stored on the server unless the user specifies to keep a copy of it.

The POP3 server can understand simple commands such as the following: o USER–enter your user ID o PASS–enter your password o QUIT–quit the POP3 server o LIST–list the messages and their size o RETR–retrieve a message, according to a message number o DELETE–delete a message, according to a message number

Since POP3 implemented email clients download mails onto the system, a user can read mails even when there is no Internet connectivity. However, because the mails are stored on the hard drive, users cannot access them from remote machines.



**IMAP Server:**

IMAP servers are similar to POP3 servers. Like POP3, IMAP handles the incoming mail. By default, the IMAP server listens to port 143 and the IMAPS (IMAP over SSL) listens on port 993.

IMAP stores emails on the mail server and allows users to view and work on their emails, as if the mails are stored in their local systems. This enables users to organize all the mails depending on their requirements. In contrast with POP3, IMAP does not move the mail server to the user’s mailbox. It acts as a remote server that stores all the user’s mails in the mail server. IMAP allows email clients to retrieve multi-purpose Internet mail extensions (MIME) parts either in the form of an entire message or in multiple bits, allowing the clients to retrieve only the text part of the mail without downloading the attachment.

This protocol stores a copy of all the emails on the server even if the user downloads them onto their system. As a result, users can access them from any computing system or device.

***

### How Email Communication Works

When a user sends an email message to any recipient, it goes through several stages before it reaches its destination.

A scenario is presented below to elaborate on these stages:

• Consider that an email user named Bob wants to send an email message to another email user named Alice. He composes a message using his email client or MUA or a web-based email like Yahoo!, specifies the email address of Alice, and hits the send button. • The email message is received by an MTA server via SMTP that decrypts the header information and searches for the domain name in Alice's email address. This method allows the MTA server to determine the destination mail server and pass on the message to the corresponding MTA. • Each time an MTA server receives a message during the mail delivery process, it modifies its header information. When it reaches the last MTA, it is transferred to the MDA, which keeps the message in Alice's mailbox. • Alice's MUA then retrieves the message using either POP3 or IMAP, and Alice is finally able to read the message.



<figure><img src="../.gitbook/assets/how email works.png" alt=""><figcaption></figcaption></figure>

***

### Understanding the Parts of an Email Message

The email message is a brief and informal text sent or received over a network. Email messages are simple text messages that can also include attachments such as image files and spreadsheets. Multiple recipients can receive email messages at a time. At present, RFC 5322 defines the Internet email message format and RFC 2045 through RFC 2049 defines multimedia content attachments—together these are called multi-purpose Internet mail extensions (MIME).

Email messages comprise three main sections:



<figure><img src="../.gitbook/assets/email compoments.png" alt=""><figcaption><p>Email components</p></figcaption></figure>

1.  **Header:**

    The message header includes the following fields: • To specifies to whom the message is addressed. Note that the “To” header does not always contain the recipient’s address. • Cc stands for “carbon copy.” This header specifies additional recipients beyond those listed in the “To” header. The difference between “To” and “Cc” is essentially connotative; some mailers also deal with them differently in generating replies. • Bcc stands for “blind carbon copy.” This header sends copies of emails to people who might not want to receive replies or appear in the headers. Blind carbon copies are popular with spammers because they confuse many inexperienced users who receive an email that does not have their address or does not appear to be for them. • From specifies the sender of the message • Reply-To specifies an address for sending replies. Though this header has many legitimate uses, it is also widely used by spammers to deflect criticism. Occasionally, a native spammer will solicit responses by email and use the “ReplyTo” header to collect them, but more often, the address specified in junk email is either invalid or that of an innocent victim. • Sender is unusual in email (“X-Sender” is usually used instead) but appears occasionally, especially in copies of Usenet posts. It should identify the sender; in the case of Usenet posts, it is a more reliable identifier than the “From” line. • Subject is a completely free-form field specified by the sender to describe the subject of the message • Date specifies the date of creation and sending of the email. If the sender’s computer omits this header, a mail server or some other machine might conceivably add it, even along the route. • MIME-Version is another MIME header that reflect MIME protocol’s version o Priority is an essentially free-form header that assigns a priority to the mail. Most of the software ignore it. Spammers often use it in an attempt to get their messages read.
2. **Message Body:** The body of the email conveys the message and sometimes includes a signature block at the end. A blank line separates the header and body. In an email, the body or text always comes after the header lines. The email body is the main message of the email that contains text, images, hyperlinks, and other data (like attachments). The email body displays separate attachments that appear in line with the text. The Internet email standard has not set any limitations on the size of an email’s body. However, individual mail servers have message size limits.
3. **Signature:** An email signature is a small amount of additional information attached at the end of the email message that consists of the name and contact details of the email sender. It can contain plain text or images.

***

## Understand Email Crime Investigation and its Steps

Email crime investigation is primarily conducted to examine the content as well as the origin of any email message that is found to be offensive or suspected to be spoofed.

**Introduction to Email Crime Investigation**

Email crime investigation involves the extraction, acquisition, analysis, and revival of email messages related to any cybercrime. Detailed analysis of email messages helps investigators gather useful evidence such as the date and time when the email message was sent, the actual IP address of the sender and recipient, and what spoofing mechanism was used. Investigators need to use many forensic tools to extract metadata from email headers. This helps them locate the criminal behind the crime and report the findings in order to prosecute them in the court of law. Investigations of criminal activities or violations of policies related to emails are similar to other kinds of computer crime investigations. Email crimes can be categorized in two ways: one committed by sending emails and the other supported by email. When criminals use spam, fake email, mail bombing, or mail storms to sell narcotics, stalk, commit fraud, or commit child abduction, it can be said that those emails support cybercrime. Let us discuss in detail the crimes committed by sending emails.

***

### Email Crimes

#### Email Spamming

Spam is unsolicited by commercial or junk email. Spam mail involves sending the 9same content to a huge number of addresses at the same time. Spamming or junk mail fills mailboxes and prevents users from accessing their regular emails.

#### Phishing

Phishing has emerged as an effective method for stealing personal and confidential data of users. It is an Internet scam that tricks users into divulging their personal and confidential information by making interesting statements and offers. Phishers can attack users by mass mailing to millions of email addresses worldwide. The phishing attack deceives and convinces the user with fake technical content along with social engineering practices. The major task for phishers is to make the victims believe that the phishing sites are legitimate.

Following are given some types of phishing attacks:

**Spear Phishing**

is when, instead of sending thousands of emails, some attackers use specialized social engineering content directed at a specific employee or a small group of employees in a specific organization to steal sensitive data such as financial information and trade secrets.<br>

**Whaling**

is a type of phishing attack that targets high-profile executives such as CEOs, CFOs, politicians, and celebrities who have complete access to confidential and highly valuable information.<br>

**Pharming**

is a social engineering technique in which an attacker executes malicious programs on a victim’s computer or server. When the victim enters any URL or domain name, it automatically redirects the victim’s traffic to a website controlled by the attacker.<br>

**Spimming**

or “spam over instant messaging” (SPIM) exploits instant messaging platforms and uses IM as a tool to spread spam. A person who generates spam over IM is called a spimmer.

#### **Mail Bombing**

Email bombing refers to the process of repeatedly sending an email message to a particular address at a specific victim’s site. In many instances, the messages will be filled with junk data aimed at consuming more system and network capacity. Multiple accounts at the target site may be abused.

#### **Mail Storms**

A mail storm occurs when computers start communicating without human intervention. The flurry of junk mail sent by an accident is a mail storm. Usage of mailing lists, autoforwarding emails, automated response, and the presence of more than one email address are the various causes of a mail storm. Malicious software code is also written to create mail storms such as the “Melissa, I-Care-For-U” message. Mail storms hinder communication systems and make them inoperable. Let us now focus on the crimes supported by emails.<br>

#### **Identity Fraud**

Identity fraud is the illegitimate retrieval and use of others’ personal data for malicious and monetary gains. Identity theft is a crime that is quickly gaining popularity.<br>

#### **Cyberstalking**

Cyberstalking is a crime where attackers harass an individual, a group, or an organization using emails or IMs. Attackers try to threaten, make false accusations, defame, slander, libel, or steal the identity of the victim/victims as a part of cyberstalking. The stalker can be someone associated with a victim or a stranger.<br>

#### **Child Abduction**

Child abduction is the offense of wrongfully removing or retaining, detaining, or concealing a child or baby. Abduction is defined as taking away a person by persuasion, fraud, or open force or violence. There are two types of child abduction: parental child abduction and abduction by a stranger. Parental child abductions are the most common type, while abduction by a stranger will be categorized as kidnapping.

***

### Steps to Investigate Email Crimes

To be able to find, extract, and analyze email-related evidence, an investigator must follow a series of defined and practiced steps. This will not only ease the process of gathering evidence but also help the investigator maintain compliance and integrity.

Some of the vital steps to be followed while conducting an email crime investigation are as follows:

1. Seizing the computer and email accounts
2. Acquiring the email data
3. Examining email messages
4. Retrieving email headers
5. Analyzing email headers
6. Recovering deleted email messages

***

#### Seizing the computer and email accounts

Get a search warrant that will grant you permission to perform on-site examination of suspect device and the email server used to send the emails.

Seize all computers and email accounts that are suspected in the crime.

You can seize the account by changing the current password of the email account. Get the current password, that you need in order to make the change, by asking the suspect or get it from the email server
