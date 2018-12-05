The Source Code for this project is used from Chromium's GQUIC Github repository. Please follow below instructions to use QUIC. 

Link to github Repository: https://chromium.googlesource.com/chromium/tools/depot_tools/

### Install depot_tools

#### Clone the depot_tools repository:

    $ git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git 

OR download source from this repository. 

Add depot_tools to the end of your PATH (you will probably want to put this in your ~/.bashrc or ~/.zshrc). Assuming you cloned depot_tools to /path/to/depot_tools:

    $ export PATH="$PATH:/path/to/depot_tools"

When cloning depot_tools to your home directory do not use ~ on PATH, otherwise gclient runhooks will fail to run. Rather, you should use either $HOME or the absolute path:

    $ export PATH="$PATH:${HOME}/depot_tools"

#### Get the code

Create a chromium directory for the checkout and change to it (you can call this whatever you like and put it wherever you like, as long as the full path has no spaces):

    $ mkdir ~/chromium && cd ~/chromium

#### Run the fetch tool from depot_tools to check out the code and its dependencies.

    $ fetch --nohooks chromium

    $ cd src

#### Install additional build dependencies

Once you have checked out the code, and assuming you're using Ubuntu, run 

    build/install-build-deps.sh

You may need to adjust the build dependencies for other distros. There are some notes at the end of this document, but we make no guarantees for their accuracy.
Run the hooks

Once you've run install-build-deps at least once, you can now run the Chromium-specific hooks, which will download additional binaries and other things you might need:

    $ gclient runhooks

Optional: You can also install API keys if you want your build to talk to some Google services, but this is not necessary for most development and testing purposes.
Setting up the build

Chromium uses Ninja as its main build tool along with a tool called GN to generate .ninja files. You can create any number of build directories with different configurations. To create a build directory, run:

    $ gn gen out/Default

    You only have to run this once for each new build directory, Ninja will update the build files as needed.
    You can replace Default with another name, but it should be a subdirectory of out.
    For other build arguments, including release settings, see GN build configuration. The default will be a debug component build matching the current host operating system and CPU.
    For more info on GN, run gn help on the command line or read the quick start guide.

#### Build the QUIC client and server
A sample server and client implementation are provided in Chromium. To use these you should first have checked out the Chromium source, and then build the binaries:

    ninja -C out/Debug quic_server quic_client

#### Prepe test data from www.example.org
Download a copy of www.example.org, which we will serve locally using the quic_server binary:

    mkdir /tmp/quic-data
    cd /tmp/quic-data
    wget -p --save-headers https://www.example.org


Manually edit index.html and adjust the headers:

        Remove (if it exists): "Transfer-Encoding: chunked"
        Remove (if it exists): "Alternate-Protocol: ..."
        Add: X-Original-Url: https://www.example.org/

#### Generate certificates
In order to run the server, you will need a valid certificate, and a private key in pkcs8 format.  If you don't have one, there are scripts you can use to generate them:

    cd net/tools/quic/certs
    ./generate-certs.sh
    cd -

In addition to the server's certificate and public key, this script will also generate a CA certificate (net/tools/quic/certs/out/2048-sha256-root.pem) which you will need to add to your OS's root certificate store in order for it to be trusted during certificate validate. For doing this on linux, please see these instructions.


### Run the QUIC server and client
#### Run the quic_server:

    ./out/Default/quic_server --quic_response_cache_dir=/home/akshay/tmp/quic-data/www.example.org --certificate_file=net/tools/quic/certs/out/leaf_cert.pem --key_file=net/tools/quic/certs/out/leaf_cert.pkcs8

#### And you should be able to successfully request the file over QUIC using quic_client:

    ./out/Default/quic_client --host=127.0.0.1 --port=6121 https://www.example.org/

Note that if you let the server's port default to 6121, you must specify the client port because it defaults to 80.
Moreover, if your local machine has multiple loopback addresses (as it would if using both IPv4 and IPv6), you have to pick a specific address.
It remains to be determined whether the latter shortcoming is a bug.

note: both the client and server are meant mainly for integration testing: neither is performant at scale!

To test the same download using chrome, 

    chrome --user-data-dir=/tmp/chrome-profile --no-proxy-server --enable-quic --origin-to-force-quic-on=www.example.org:443 --host-resolver-rules='MAP www.example.org:443 127.0.0.1:6121' https://www.example.org



