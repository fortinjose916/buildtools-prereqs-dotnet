FROM mcr.microsoft.com/dotnet-buildtools/prereqs:windowsservercore-ltsc2019-helix-amd64

# Install git
ENV GIT_VERSION=2.32.0
ENV GIT_INSTALLER=MinGit-${GIT_VERSION}-64-bit.zip

RUN curl -SL --output %TEMP%\%GIT_INSTALLER% https://github.com/git-for-windows/git/releases/download/v%GIT_VERSION%.windows.1/%GIT_INSTALLER% \
    && mkdir C:\git \
    && tar -C C:\git -zxf %TEMP%\%GIT_INSTALLER% \
    && setx PATH "%PATH%;C:\git\cmd"

# fix certificates for python to be able to download emscripten files
RUN certutil -generateSSTFromWU roots.sst && certutil -addstore -f root roots.sst && del roots.sst

# Install Emscripten toolchain
ENV EMSCRIPTEN_VERSION=2.0.23
ENV EMSCRIPTEN_PATH="C:\emscripten"
ENV EMSDK_PATH="C:\emscripten\emsdk"

RUN mkdir %EMSCRIPTEN_PATH% \
    && cd %EMSCRIPTEN_PATH% \
    && git clone https://github.com/emscripten-core/emsdk.git %EMSDK_PATH%
RUN cd %EMSDK_PATH% \
    && .\emsdk install %EMSCRIPTEN_VERSION%-upstream  \
    && .\emsdk activate %EMSCRIPTEN_VERSION%-upstream

# install Node JS
ENV NODE_VERSION 16.3.0

RUN curl -SL --output %TEMP%\nodejs.msi https://nodejs.org/dist/v%NODE_VERSION%/node-v%NODE_VERSION%-x64.msi
RUN msiexec /i %TEMP%\nodejs.msi /quiet /passive /qn /norestart

# install jsvu and engines
RUN npm install jsvu -g
RUN npm exec -c "jsvu --os=win64 --engines=v8,spidermonkey"
RUN setx PATH "%PATH%;%USERPROFILE%\.jsvu"