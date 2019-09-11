# FaceDetection4AndroidJava

This is face detection library for Android with Java code.

It is easy to implement face detection With Firebase ML Kit's face detection API, you can detect faces in an image, identify key facial features, and get the contours of detected faces.

You can detect following things with ML Kit's face detection API
- Face
- Smiling probability
- Nose
- Left Eye and Right Eye
- Left Eyebrow top/bottom and Right Eyebrow top/bottom
- Upper Lip Top and Upper Lip Bottom and Lower Lip Top and Lower Lip Bottom
- Left Cheek and Right Cheek
- Left Ear and Right Ear
- FaceContour

Usage
1 Add below code in app level gradle
```
implementation 'com.google.firebase:firebase-ml-vision:23.0.0'
implementation 'com.google.firebase:firebase-ml-vision-face-model:18.0.0'
```
2 You need to add FaceDetectionProcessor in your activity. Add below code in your FaceDetectionActivity
```
cameraSource.setMachineLearningFrameProcessor(new FaceDetectionProcessor(getResources()));
```
3 draw method is used to draw face inside FaceGraphic class
```Java
@Override
    public void draw(Canvas canvas) {
        FirebaseVisionFace face = firebaseVisionFace;
        if (face == null) {
            return;
        }

        // Draws a circle at the position of the detected face, with the face's track id below.
        // An offset is used on the Y axis in order to draw the circle, face id and happiness level in the top area
        // of the face's bounding box
        float x = translateX(face.getBoundingBox().centerX());
        float y = translateY(face.getBoundingBox().centerY());
        canvas.drawCircle(x, y - 4 * ID_Y_OFFSET, FACE_POSITION_RADIUS, facePositionPaint);
        canvas.drawText("id: " + face.getTrackingId(), x + ID_X_OFFSET, y - 3 * ID_Y_OFFSET, idPaint);
        int happiness = (int)(face.getSmilingProbability()*100);
        canvas.drawText(
                "happiness: " + happiness,
                x + ID_X_OFFSET * 3,
                y - 2 * ID_Y_OFFSET,
                idPaint);
        if (facing == CameraSource.CAMERA_FACING_FRONT) {
            canvas.drawText(
                    "right eye: " + String.format("%.2f", face.getRightEyeOpenProbability()),
                    x - ID_X_OFFSET,
                    y,
                    idPaint);
            canvas.drawText(
                    "left eye: " + String.format("%.2f", face.getLeftEyeOpenProbability()),
                    x + ID_X_OFFSET * 6,
                    y,
                    idPaint);
        } else {
            canvas.drawText(
                    "left eye: " + String.format("%.2f", face.getLeftEyeOpenProbability()),
                    x - ID_X_OFFSET,
                    y,
                    idPaint);
            canvas.drawText(
                    "right eye: " + String.format("%.2f", face.getRightEyeOpenProbability()),
                    x + ID_X_OFFSET * 6,
                    y,
                    idPaint);
        }

        // Draws a bounding box around the face.
        float xOffset = scaleX(face.getBoundingBox().width() / 2.0f);
        float yOffset = scaleY(face.getBoundingBox().height() / 2.0f);
        float left = x - xOffset;
        float top = y - yOffset;
        float right = x + xOffset;
        float bottom = y + yOffset;
        canvas.drawRect(left, top, right, bottom, boxPaint);

        // draw landmarks
        drawLandmarkPosition(canvas, face, FirebaseVisionFaceLandmark.MOUTH_BOTTOM);
        drawLandmarkPosition(canvas, face, FirebaseVisionFaceLandmark.LEFT_CHEEK);
        drawLandmarkPosition(canvas, face, FirebaseVisionFaceLandmark.LEFT_EAR);
        drawLandmarkPosition(canvas, face, FirebaseVisionFaceLandmark.MOUTH_LEFT);
        drawLandmarkPosition(canvas, face, FirebaseVisionFaceLandmark.LEFT_EYE);
        drawBitmapOverLandmarkPosition(canvas, face, FirebaseVisionFaceLandmark.NOSE_BASE);
        drawLandmarkPosition(canvas, face, FirebaseVisionFaceLandmark.RIGHT_CHEEK);
        drawLandmarkPosition(canvas, face, FirebaseVisionFaceLandmark.RIGHT_EAR);
        drawLandmarkPosition(canvas, face, FirebaseVisionFaceLandmark.RIGHT_EYE);
        drawLandmarkPosition(canvas, face, FirebaseVisionFaceLandmark.MOUTH_RIGHT);
    }
```
Output:

![alt text](https://github.com/1986webdeveloper/FaceDetection4AndroidKotlin/blob/master/ezgif-4-72e974aee956.gif)
