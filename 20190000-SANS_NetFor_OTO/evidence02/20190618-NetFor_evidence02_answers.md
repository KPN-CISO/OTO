1. What is Ann’s email address?

sneakyg33k@aol.com

By looking at the SMTP command exchange in the PCAP, the forensicator can see the MAIL FROM: ... command. However, this can be faked and is less reliable than looking at the USER authentication field. This is a Base64-encoded username. The Base64 decode of "c25lYWt5ZzMza0Bhb2wuY29t" matches the MAIL FROM: address.

2. What is Ann’s email password?

By examining the SMTP PASS command, a Base64 decode of "NTU4cjAwbHo=" yields '558r00lz'.

3. What is Ann’s secret lover’s email address?

mistersecretx@aol.com. Ann mails her 'secret558user1' from exercise 1 stating that she cannot make it, and afterwards mails her 'secret lover' with instructions where to meet up and what to bring.

4. What two items did Ann tell her secret lover to bring?

"fake passport and bathing suit". This is visible in cleartext by examining the DATA segment of the SMTP exchange.

5. What is the NAME of the attachment Ann sent to her secret lover?

secretrendezvous.docx. This can be deduced by looking at the MIME boundaries until a Content-Type of 'application/octet-stream' is found, implying that the next MIME segment contains a file. As part of the metadata, a filename of 'secretrendezvous.docx' is visible.

6. What is the MD5sum of the attachment Ann sent to her secret lover?

9e423e11db88f01bbff81172839e1923  secretrendezvous.docx

Extracting the attachment can be done in multiple ways:

6a) Save the RAW data, and use the 'dd' command to strip off the MIME boundaries until only the Base64-encoded payload is left.

6b) Copying and pasting the RAW data and piping it to 'cat -|xxd -r -p > secretrendezvous.docx' and Base64-decoding that.

6c) Saving the SMTP exchange starting at the MAIL FROM: part as an .EML file. This can then be opened in a mail-client and the attachment can be saved. However, this may pose risks if the e-mail cannot be trusted (malware, etc.)

Please note that the payload may have CRLF mismatches and must be decoded by either first converting it with 'dos2unix', or by telling the base64 command-line tool to ignore ('-i' flag) garbage.

7. In what CITY and COUNTRY is their rendez-vous point?

Playa del Carmen, 77780, Mexico. This can be seen when opening the DOCX. A forensicator should only do this when (s)he trusts the document not to contain malware etc., and in a sandbox if that cannot be determined.

8. What is the MD5sum of the image embedded in the document?

aadeace50997b1ba24b09ac2ef1940b7  image1.png

The easiest methods are unpacking the DOCX archive with 'unzip', as Office 2007+ documents are a ZIP file. This will extract an image1.png file in the /word/media/ folder. Alternatively, opening the document in OpenOffice/LibreOffice lets the forensicator right-click -> save the image.