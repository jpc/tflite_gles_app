Building tensorflow (crosscompilation):

    bazelisk build -s -c opt --config=elinux_aarch64 //tensorflow/lite:libtensorflowlite.so
    bazelisk build -s -c opt --config=elinux_aarch64 --copt="-DMESA_EGL_NO_X11_HEADERS" --copt="-DEGL_NO_X11" tensorflow/lite/delegates/gpu:libtensorflowlite_gpu_delegate.so

Copy the libraries:

    cp bazel-bin/tensorflow/lite/libtensorflowlite.so bazel-bin/tensorflow/lite/delegates/gpu/libtensorflowlite_gpu_delegate.so /usr/local/lib

To build the demo app you need `flatbuffers` header files (I installed from source) as well as the tensorflow sources (they don't seem
to have an official way to install the headers files...) I did:

    ln -s /home/user/tensorflow/tensorflow /usr/local/include

Afterwards you should be fetch the demo repository (https://github.com/jpc/tflite_gles_app) and run:

    git clone git@github.com:jpc/tflite_gles_app.git
    cd tflite_gles_app/gl2facemesh
    make -j8 TARGET_ENV=wayland TFLITE_DELEGATE=GPU_DELEGATEV2

The CPU version:
    make -j8 TARGET_ENV=wayland

Running the app (GStreamer errors are mostly ignored right now so GST_DEBUG is crucial):

    GST_DEBUG=*:WARN ./gl2facemesh -v input480p.mkv'
