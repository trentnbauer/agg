# Stirling PDF

[Link to App](https://pdf.xfgn.dev)

[Link to GitHub or Website](https://github.com/Frooodle/Stirling-PDF)

This is a powerful locally hosted web based PDF manipulation tool using docker that allows you to perform various operations on PDF files, such as splitting merging, converting, reorganizing, adding images, rotating, compressing, and more. This locally hosted web application started as a 100% ChatGPT-made application and has evolved to include a wide range of features to handle all your PDF needs.

Stirling PDF makes no outbound calls for any record keeping or tracking.

All files and PDFs are either purely client side, in server memory only during the execution of the task or within a temporay file only for execution of the task. Any file which has been downloaded by the user will have already been deleted from the server by that time.

This app is hosted on Cocoa as a docker container

{% @github-files/github-code-block url="https://github.com/trentnbauer/agg.local/blob/main/docker-compose/stirlingpdf.yml" %}

### Instance 1

| Port | Purpose |
| ---- | ------- |
| 8233 | WebUI   |

| Host Volume               | Container Volume                       | Purpose             |
| ------------------------- | -------------------------------------- | ------------------- |
| stirlingpdf\_trainingdata | /usr/share/tesseract-ocr/4.00/tessdata | OCR data            |
| stirlingpdf\_config       | /config                                | configuration files |

\
