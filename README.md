# Glucoshema: Non-Invasive Blood Component Measurement on a Smartphone

Glucoshema is an Android app that estimates **blood glucose** and **hemoglobin** levels from a 15-second fingertip video, without drawing blood. It records the video using either the phone's flashlight or an external NIR LED as the light source. It then extracts a photoplethysmogram (PPG) signal from the frames and runs a deep neural network on the device to predict glucose (mmol/L) and hemoglobin (g/dL).

This repository accompanies the paper *Mobile Application to Collect Data and Measure Blood Component Level in a Non-Invasive Way* (ICCIT 2023).

<p align="center">
  <img src="S1.png" width="45%" alt="App screens"/>
  <img src="S2.png" width="45%" alt="App screens"/>
</p>

## Highlights

- **Guided fingertip recording:** a camera preview with automatic 15-second capture, so users don't have to stop the recording themselves.
- **Two light sources:** camera presets (ISO, exposure time, torch) for the phone's flashlight and for an external NIR LED board, to keep video quality stable under different lighting.
- **On-device prediction:** PPG extraction, feature extraction and DNN inference run inside the app through [Chaquopy](https://chaquo.com/chaquopy/), with no server needed.
- **Simple interface:** designed for people with anemia or diabetes, including those with little smartphone experience.
- **User accounts and history:** Firebase Authentication for sign-up and login, and Firebase Realtime Database for storing user details and past results.
- **Tested across devices:** Samsung, Realme, Vivo and Redmi phones.

## How It Works

1. **User information:** the user enters their name, age, gender, height and weight.
2. **Fingertip video:** the user places their index finger over the flashlight or the NIR LED device and starts recording. The app captures a 15-second Full HD video and saves it locally.
3. **PPG extraction:** the app extracts frames from the video, generates a PPG signal and filters it.
4. **Feature extraction:** the app computes features from the PPG waveform.
5. **Prediction:** pretrained DNN models (`DNN_model_Gl.h5`, `DNN_model_Hb.h5`) estimate glucose and hemoglobin from these features.
6. **Result and history:** the app displays the estimated levels and saves them to the user's record.

![Activity flow](https://github.com/sadman15019/CSE-3200-final/assets/66266178/88b1cd4b-fcf4-4dd0-86a0-150c17032727)

The prediction model follows Haque et al., *"A novel technique for non-invasive measurement of human blood component levels from fingertip video using DNN based models,"* IEEE Access, 2021.

## Results

The study had 15 participants (11 male, 4 female, aged 20–25). Hemoglobin was estimated with the app using the phone's flashlight and compared with the invasive cyanmethemoglobin method. The app achieved **R² = 0.81** and **MAE = 0.68 g/dL**.

## Tech Stack

| Layer | Tools |
|---|---|
| Android app | Kotlin, CameraX (with Camera2 interop), View Binding |
| On-device Python | Chaquopy, NumPy, pandas, SciPy, OpenCV |
| Model | TensorFlow 2.1 / Keras 2.3.1 |
| Backend | Firebase Authentication, Firebase Realtime Database |

## Project Structure

```
app/
├── src/main/java/com/app/mycamapp/   # Kotlin activities
│   ├── MainActivity.kt               # camera preview, light-source presets, 15 s recording
│   ├── upload.kt                     # runs the Python pipeline and saves results
│   ├── result.kt                     # shows glucose and hemoglobin levels
│   ├── record.kt, user_rec.kt        # past results
│   ├── login.kt, signup.kt           # authentication
│   └── DataUpdate.kt                 # user information
└── src/main/python/New/Glucose/
    ├── scripts/
    │   ├── generate_dataset.py       # frames → PPG → features → prediction
    │   ├── model_train.py            # model training
    │   └── DNN_model_Gl.h5, DNN_model_Hb.h5
    └── GA/                           # genetic-algorithm feature selection
```

## Getting Started

**Requirements:** Android Studio, an Android device running Android 5.0 (API 21) or later, and a Firebase project.

1. Clone the repository:
   ```bash
   git clone https://github.com/sadman15019/Mobile-Application-to-Measure-Blood-Component-Level-in-a-Non-Invasive-Way.git
   ```
2. Open the project in Android Studio and let Gradle sync. Chaquopy installs the Python packages listed in `app/build.gradle` during the build.
3. To use your own Firebase project, replace `app/google-services.json` with its config file, then enable Email/Password sign-in and the Realtime Database.
4. Build and run the app on a physical device. The device's camera needs a flashlight.

## Authors

- **Humaira Anzum Neha**, Khulna University of Engineering & Technology (KUET)
- **Sadman Sakib**, KUET
- **Farhan Sadaf**, KUET
- **S. M. Taslim Uddin Raju**, KUET

## Citation

```bibtex
@inproceedings{neha2023mobile,
  title     = {Mobile Application to Collect Data and Measure Blood Component Level in a Non-Invasive Way},
  author    = {Neha, Humaira and Sakib, Sadman and Sadaf, Farhan and Raju, S. M. Taslim Uddin},
  booktitle = {2023 26th International Conference on Computer and Information Technology (ICCIT)},
  address   = {Cox's Bazar, Bangladesh},
  year      = {2023},
  publisher = {IEEE}
}
```

## Disclaimer

This app is a research prototype, not a medical device. Its estimates should not be used for diagnosis or treatment.

## License

Released under the [MIT License](LICENSE).
