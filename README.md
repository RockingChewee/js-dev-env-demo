# js-dev-env-demo
JavaScript development environment Pluralsight course by Cory House

Initial tooling setup
==================
* __Git client__<br/>
  Download from: https://git-scm.com/download/win (cancel the auto-started download)<br/>
  Pick: _64-bit Git for Windows Portable_<br/>
  Extract to: _C:\Tools\PortableGit_<br/>

* __Node.js__<br/>
  Download from: https://nodejs.org/en/download/<br/>
  Pick: _Windows Binary (.zip) 64-bit_<br/>
  Extract to versioned folder: _C:\Tools\JavaScript\node-v6.12.0-win-x64_<br/>

* __Heroku CLI__<br/>
  Download from: https://devcenter.heroku.com/articles/heroku-cli<br/>
  Pick: _Other installation methods / Tarballs / Windows (x64)_<br/>
  Extract to: _C:\Tools\heroku_<br/>
  > Extract the archive contents by running 7-zip as administrator. Otherwise the symbolic links won't be created.<br/>

* __OpenShift CLI__<br/>
  Download from: https://github.com/openshift/origin/releases<br/>
  Pick: openshift-origin-client-tools-vX.XX.X-YYYYYYY-windows.zip<br/>
  Extract to: _C:\Tools\OpenShift_<br/>
  > More details on OpenShift CLI installation can be found here https://blog.openshift.com/installing-oc-tools-windows/<br/>

* __VSCode__<br/>
  Download from: https://code.visualstudio.com/download<br/>
  Pick: _Windows .zip 64 bit_<br/>
  Extract to versioned folder: _C:\Tools\JavaScript\VSCode-win32-x64-1.26.0_<br/>
  - Create the project specific shortcut to __Code.exe__ by supplying options for _user-data_, _extensions_ and _project folder_:

    `C:\Tools\JavaScript\VSCode-win32-x64-1.26.0\Code.exe --user-data-dir "userdata" --extensions-dir "ext" "../Projects/bpm-portal`

  - Launch the modified shortcut and download the following _extensions_ via VSCode interface:
    - _EditorConfig for VS Code_ - honors the instructions set at __.editorconfig__ file from the project root.
    - _vscode-icons_ - beautifies the icons in the file explorer view.
    - _Log File Highlighter_ - adds color highlighting to log files to make it easier to follow the flow of log events and identify problems.

  - Paste the following _JSON_ to:
    > VSCode &rightarrow; File &rightarrow; Preferences &rightarrow; Settings &rightarrow; User Settings:
    ```
    {
      "git.path": "C:\\Tools\\PortableGit\\bin\\git.exe",
      "workbench.iconTheme": "vscode-icons",
      "terminal.integrated.shell.windows": "C:\\Tools\\PortableGit\\bin\\bash.exe",
      "terminal.integrated.shellArgs.windows": [
        "-c",
        "export PATH=/C/Tools/PortableGit/bin:/C/Tools/PortableGit/usr/bin:/C/Tools/JavaScript/node-v6.12.0-win-x64:/C/Tools/heroku/bin:/C/Tools/OpenShift:$PATH;export http_proxy=http://<username:password>@proxy-w-app.danskenet.net:80;export https_proxy=http://<username:password>@proxy-w-app.danskenet.net:80;export NODE_PATH=/C/Tools/JavaScript/node-v6.12.0-win-x64/node_modules;export NODE_ENV=development;export BABEL_ENV=development;export DEPLOYMENT_VERSION=latest-commit;bash"
      ],
      "window.zoomLevel": 0
    }
    ```

* __Test__<br/>
  Launch the integrated terminal within VSCode (_Ctrl+`_) and execute the following commands to make sure all the tools are reachable:
  ```
  git --version
  node -v
  npm -v
  heroku -v
  oc version
  ```
  > Corporate OpenShift instance may show an error after the line with the version, but this is expected.


Git repository access setup
==================
* In corporate environment, in order to be able to access the local __Stash/BitBucket__ instance, the _Root CA_ certificate needs to be added into Git client truststore:
  - Export the _Danske Bank Root CA_ certificate in __Base-64 encoded X.509__ format from https://stash.danskenet.net/.
  - Append it the captured certificate to the end of _C:/Tools/PortableGit/mingw64/ssl/certs/ca-bundle.crt_ file via any text editor.

* Clone the wanted repository while in the projects root directory:

  `git clone https://BB6545@stash.danskenet.net/scm/bpm/bpm-portal.git`

* Navigate to the cloned project directory and execute the following commands to update the project specific git configuration:
  - __Alternative 1:__ For corporate projects with git repositories managed by the local Stash/BitBucket instance
    ```
    git config --local user.name "Aleksandr Fokin"
    git config --local user.email "...@danskebank.lt"
    git config --local credential.username "BB6545"
    ```

  - __Alternative 2:__ For public projects with git repositories managed by GitHub
    ```
    git config --local user.name "Aleksandr Fokin"
    git config --local user.email "...@gmail.com"
    git config --local credential.username "aleksf0"
    ```

- Upon the first authentication, the credentials in both cases are saved in __Windows Credentials Manager__ and can be viewed at:

  > Control Panel &rightarrow; User Accounts &rightarrow; Credential Manager &rightarrow; Windows Credentials &rightarrow; Generic Credentials


Initial .npmrc configuration
==================
Create the __.npmrc__ file with the following contents in the project root:

```
# Use the exact dependencies specified in package.json
save-exact=true

# Corporate specific npm module registry.
# Comment-out for non-corporate projects.
registry=http://artifactory.danskenet.net/artifactory/api/npm/joined-npm-build

# Path to the local node-sass preprocessor binary used in development.
# It is overridden by the environment variable in server environment.
SASS_BINARY_PATH=${PWD}/mods/bin/node-sass/v4.5.2/win32-x64-48_binding.node
```
