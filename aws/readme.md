# Connect to the instance
```
ssh -i "workshop.pem" ec2-user@ec2-xxxxx.compute-1.amazonaws.com
```

# npm & create-react-app & amplify cli install
```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash
. ~/.nvm/nvm.sh
nvm install node
npm install -g create-react-app
npm install -g @aws-amplify/cli
```

# Required software and libraries
```
curl -o- -L https://yarnpkg.com/install.sh | bash
sudo yum -y install xorg-x11-server-Xvfb libXcomposite.x86_64 libXcursor.x86_64 libXi.x86_64 libXtst.x86_64 gdk-pixbuf2 gtk3.x86_64 libXScrnSaver alsa-lib-devel
```

