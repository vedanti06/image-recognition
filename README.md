# image-recognition

AWS IaaS application which automatically scales-out and scales-in the elastic instances based on the inputs. The user inputs the image request, that are sent as post messages and utilizes AWS services that uses deep learning classifiers to recognize the image and display the result back to the user. 

The user uploads multiple images to the hosted website, triggering a POST request.
The POST request is directed to an Nginx server acting as a reverse proxy for a Gunicorn web server. Nginx serves static HTML and CSS files directly and routes dynamic requests to Gunicorn.
Each image has a unique identifier (image-id), and uploads the images to an S3 Input Bucket with image-id as the key and the image file as the value.
The image-ids are pushed to an SQS Request Queue and initiates the autoscaling of App Tier instances based on the number of queued requests, with maximum instances and images per instance.
Each new App Tier EC2 instance boots up and automatically starts polling the SQS Request Queue for image-ids.
Once an image-id is retrieved, the image from the S3 Input Bucket, processes it using the deep learning classifier, and generates a prediction result.
The result is inputted to an S3 Output Bucket and deletes the processed image-id from the request queue.
