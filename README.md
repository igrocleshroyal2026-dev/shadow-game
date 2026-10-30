name: Build APK
on: [push, workflow_dispatch]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v4
    - name: Setup Node.js
      uses: actions/setup-node@v4
      with:
        node-version: '20'
    - name: Setup Java
      uses: actions/setup-java@v4
      with:
        distribution: 'zulu'
        java-version: '17'
    - name: Install Cordova
      run: npm install -g cordova
    - name: Build App
      run: |
        cordova create myapp com.example.shadow "Shadow Fight"
        cp index.html myapp/www/index.html
        cd myapp
        cordova platform add android
        cordova build android
    - name: Upload APK
      uses: actions/upload-artifact@v4
      with:
        name: shadow-game-apk
        path: myapp/platforms/android/app/build/outputs/apk/debug/app-debug.apk
