# Linux setup

## 0. reference

https://github.com/opencv/dldt/blob/2019/inference-engine/README.md

## 1. source code

https://github.com/opencv/dldt

```bash
cd WORK_DIR
git clone https://github.com/opencv/dldt.git

cd dldt/inference-engine/thirdparty
git clone https://github.com/opencv/ade.git
```

## 2. install dependencies

https://github.com/opencv/dldt/blob/2019/inference-engine/install_dependencies.sh

```bash
sudo apt install -y \
    build-essential \
    cmake \
    curl \
    wget \
    libssl-dev \
    ca-certificates \
    git \
    libboost-regex-dev \
    gcc-multilib \
    g++-multilib \
    libgtk2.0-dev \
    pkg-config \
    unzip \
    automake \
    libtool \
    autoconf \
    libcairo2-dev \
    libpango1.0-dev \
    libglib2.0-dev \
    libgtk2.0-dev \
    libswscale-dev \
    libavcodec-dev \
    libavformat-dev \
    libgstreamer1.0-0 \
    gstreamer1.0-plugins-base \
    libusb-1.0-0-dev \
    libopenblas-dev

# ubuntu 18.04
sudo apt install -y libpng-dev

# ubuntu 16.04
sudo apt install -y libpng12-dev

```

## 3. build

```bash
cd WORK_DIR
mkdir build
cd build

cmake -DCMAKE_BUILD_TYPE=Release ..
make -j8
```

## 4. test

### a. download model

download SSD caffe model from this link

https://github.com/weiliu89/caffe/tree/ssd
```
deploy.prototxt
VGG_VOC0712_SSD_300x300_iter_120000.caffemodel
```

### b. convert model

```bash
cd dldt-2019_R1.0.1/model-optimizer
python3 ./mo_caffe.py --input_model ~/tmp/VGG_VOC0712_SSD_300x300_iter_120000.caffemodel --input_proto ~/tmp/deploy.prototxt  --output_dir ./
```

### c. run sample application

**inference on CPU**

```bash
./object_detection_sample_ssd -i ~/tmp/cat.jpg -m ~/tmp/VGG_VOC0712_SSD_300x300_iter_120000.xml
```

***inference on GPU**

to use GPU, need install intel opencl compute runtime driver. refer to

https://github.com/mintaka33/vaapi-opencl-interop/blob/master/README.md

```bash
./object_detection_sample_ssd -i ~/tmp/cat.jpg -m ~/tmp/VGG_VOC0712_SSD_300x300_iter_120000.xml -d GPU
```

 gpu inference call stack
 
```
libclDNN64.so!cl::CommandQueue::enqueueNDRangeKernel(const cl::CommandQueue * const this, const cl::Kernel & kernel, const cl::NDRange & offset, const cl::NDRange & global, const cl::NDRange & local, const cl::vector * events, cl::Event * event) (/home/fresh/data/work/intel_openvino/source/dldt-2019_R1.0.1/inference-engine/thirdparty/clDNN/common/khronos_ocl_clhpp/cl2.hpp:7842)
libclDNN64.so!cldnn::gpu::gpu_toolkit::enqueue_kernel(cldnn::gpu::gpu_toolkit * const this, const cl::Kernel & kern, const cl::NDRange & global, const cl::NDRange & local, const std::vector<cldnn::refcounted_obj_ptr<cldnn::event_impl>, std::allocator<cldnn::refcounted_obj_ptr<cldnn::event_impl> > > & deps) (/home/fresh/data/work/intel_openvino/source/dldt-2019_R1.0.1/inference-engine/thirdparty/clDNN/src/gpu/ocl_toolkit.cpp:177)
libclDNN64.so!cldnn::gpu::kernel::run(const cldnn::gpu::kernel * const this, const kernel_selector::cl_kernel_data & kernel_data, const std::vector<cldnn::refcounted_obj_ptr<cldnn::event_impl>, std::allocator<cldnn::refcounted_obj_ptr<cldnn::event_impl> > > & dependencies, const cldnn::gpu::kernel::kernel_arguments_data & args) (/home/fresh/data/work/intel_openvino/source/dldt-2019_R1.0.1/inference-engine/thirdparty/clDNN/src/gpu/kernel.cpp:246)
libclDNN64.so!cldnn::gpu::typed_primitive_gpu_impl<cldnn::reorder>::execute_impl(cldnn::gpu::typed_primitive_gpu_impl<cldnn::reorder> * const this, const std::vector<cldnn::refcounted_obj_ptr<cldnn::event_impl>, std::allocator<cldnn::refcounted_obj_ptr<cldnn::event_impl> > > & events, cldnn::typed_primitive_inst<cldnn::reorder> & instance) (/home/fresh/data/work/intel_openvino/source/dldt-2019_R1.0.1/inference-engine/thirdparty/clDNN/src/gpu/primitive_gpu_base.h:159)
libclDNN64.so!cldnn::typed_primitive_impl<cldnn::reorder>::execute(cldnn::typed_primitive_impl<cldnn::reorder> * const this, const std::vector<cldnn::refcounted_obj_ptr<cldnn::event_impl>, std::allocator<cldnn::refcounted_obj_ptr<cldnn::event_impl> > > & event, cldnn::primitive_inst & instance) (/home/fresh/data/work/intel_openvino/source/dldt-2019_R1.0.1/inference-engine/thirdparty/clDNN/src/include/primitive_inst.h:170)
libclDNN64.so!cldnn::primitive_inst::execute(cldnn::primitive_inst * const this, const std::vector<cldnn::refcounted_obj_ptr<cldnn::event_impl>, std::allocator<cldnn::refcounted_obj_ptr<cldnn::event_impl> > > & events) (/home/fresh/data/work/intel_openvino/source/dldt-2019_R1.0.1/inference-engine/thirdparty/clDNN/src/primitive_inst.cpp:66)
libclDNN64.so!cldnn::network_impl::execute_primitive(cldnn::network_impl * const this, const std::shared_ptr<cldnn::primitive_inst> & primitive, const std::vector<cldnn::refcounted_obj_ptr<cldnn::event_impl>, std::allocator<cldnn::refcounted_obj_ptr<cldnn::event_impl> > > & events) (/home/fresh/data/work/intel_openvino/source/dldt-2019_R1.0.1/inference-engine/thirdparty/clDNN/src/network.cpp:499)
libclDNN64.so!cldnn::network_impl::execute(cldnn::network_impl * const this, const std::vector<cldnn::refcounted_obj_ptr<cldnn::event_impl>, std::allocator<cldnn::refcounted_obj_ptr<cldnn::event_impl> > > & events) (/home/fresh/data/work/intel_openvino/source/dldt-2019_R1.0.1/inference-engine/thirdparty/clDNN/src/network.cpp:365)
libclDNN64.so!<lambda()>::operator()(void) const(const <lambda()> * const __closure) (/home/fresh/data/work/intel_openvino/source/dldt-2019_R1.0.1/inference-engine/thirdparty/clDNN/src/cldnn.cpp:585)
libclDNN64.so!std::_Function_handler<void(), cldnn_execute_network(cldnn_network, cldnn_event_impl**, size_t, cldnn_status*)::<lambda()> >::_M_invoke(const std::_Any_data &)(const std::_Any_data & __functor) (/usr/include/c++/7/bits/std_function.h:316)
libinference_engine.so!std::function<void ()>::operator()() const(const std::function<void()> * const this) (/usr/include/c++/7/bits/std_function.h:706)
libclDNN64.so!exception_handler(int, int*, std::function<void ()>)(cldnn_status default_error, cldnn_status * status, std::function<void()> func) (/home/fresh/data/work/intel_openvino/source/dldt-2019_R1.0.1/inference-engine/thirdparty/clDNN/src/include/api_impl.h:116)
libclDNN64.so!cldnn_execute_network(cldnn_network network, cldnn_event * dependencies, size_t deps_num, cldnn_status * status) (/home/fresh/data/work/intel_openvino/source/dldt-2019_R1.0.1/inference-engine/thirdparty/clDNN/src/cldnn.cpp:575)
libclDNNPlugin.so!cldnn::network::execute[abi:cxx11](std::vector<cldnn::event, std::allocator<cldnn::event> > const&) const::{lambda(int*)#1}::operator()(int*) const(const cldnn::network::<lambda(cldnn::status_t*)> * const __closure, cldnn::status_t * status) (/home/fresh/data/work/intel_openvino/source/dldt-2019_R1.0.1/inference-engine/thirdparty/clDNN/api/CPP/network.hpp:268)
libclDNNPlugin.so!std::_Function_handler<void (int*), cldnn::network::execute(std::vector<cldnn::event, std::allocator<cldnn::event> > const&) const::{lambda(int*)#1}>::_M_invoke(std::_Any_data const&, int*&&)(const std::_Any_data & __functor,  __args#0) (/usr/include/c++/7/bits/std_function.h:316)
libclDNNPlugin.so!std::function<void (int*)>::operator()(int*) const(const std::function<void(int*)> * const this,  __args#0) (/usr/include/c++/7/bits/std_function.h:706)
libclDNNPlugin.so!cldnn::check_status<void>(std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> >, std::function<void (int*)>)(std::__cxx11::string err_msg, std::function<void(int*)> func) (/home/fresh/data/work/intel_openvino/source/dldt-2019_R1.0.1/inference-engine/thirdparty/clDNN/api/CPP/cldnn_defs.h:227)
libclDNNPlugin.so!cldnn::network::execute[abi:cxx11](std::vector<cldnn::event, std::allocator<cldnn::event> > const&) const(const cldnn::network * const this, const std::vector<cldnn::event, std::allocator<cldnn::event> > & dependencies) (/home/fresh/data/work/intel_openvino/source/dldt-2019_R1.0.1/inference-engine/thirdparty/clDNN/api/CPP/network.hpp:266)
libclDNNPlugin.so!CLDNNPlugin::CLDNNInferRequest::execAndParse(CLDNNPlugin::CLDNNInferRequest * const this) (/home/fresh/data/work/intel_openvino/source/dldt-2019_R1.0.1/inference-engine/src/cldnn_engine/cldnn_infer_request.cpp:419)
libclDNNPlugin.so!CLDNNPlugin::CLDNNInferRequest::InferImpl(CLDNNPlugin::CLDNNInferRequest * const this) (/home/fresh/data/work/intel_openvino/source/dldt-2019_R1.0.1/inference-engine/src/cldnn_engine/cldnn_infer_request.cpp:536)
libclDNNPlugin.so!InferenceEngine::InferRequestInternal::Infer(InferenceEngine::InferRequestInternal * const this) (/home/fresh/data/work/intel_openvino/source/dldt-2019_R1.0.1/inference-engine/src/inference_engine/cpp_interfaces/impl/ie_infer_request_internal.hpp:79)
libclDNNPlugin.so!InferenceEngine::AsyncInferRequestThreadSafeDefault::AsyncInferRequestThreadSafeDefault(std::shared_ptr<InferenceEngine::InferRequestInternal>, std::shared_ptr<InferenceEngine::ITaskExecutor> const&, std::shared_ptr<InferenceEngine::TaskSynchronizer> const&, std::shared_ptr<InferenceEngine::ITaskExecutor> const&)::{lambda()#1}::operator()() const(const InferenceEngine::AsyncInferRequestThreadSafeDefault::<lambda()> * const __closure) (/home/fresh/data/work/intel_openvino/source/dldt-2019_R1.0.1/inference-engine/src/inference_engine/cpp_interfaces/impl/ie_infer_async_request_thread_safe_default.hpp:95)
libclDNNPlugin.so!std::_Function_handler<void (), InferenceEngine::AsyncInferRequestThreadSafeDefault::AsyncInferRequestThreadSafeDefault(std::shared_ptr<InferenceEngine::InferRequestInternal>, std::shared_ptr<InferenceEngine::ITaskExecutor> const&, std::shared_ptr<InferenceEngine::TaskSynchronizer> const&, std::shared_ptr<InferenceEngine::ITaskExecutor> const&)::{lambda()#1}>::_M_invoke(std::_Any_data const&)(const std::_Any_data & __functor) (/usr/include/c++/7/bits/std_function.h:316)
libinference_engine.so!std::function<void ()>::operator()() const(const std::function<void()> * const this) (/usr/include/c++/7/bits/std_function.h:706)
libinference_engine.so!InferenceEngine::Task::runNoThrowNoBusyCheck(InferenceEngine::Task * const this) (/home/fresh/data/work/intel_openvino/source/dldt-2019_R1.0.1/inference-engine/src/inference_engine/cpp_interfaces/ie_task.cpp:35)
libinference_engine.so!InferenceEngine::Task::runWithSynchronizer(InferenceEngine::Task * const this, InferenceEngine::TaskSynchronizer::Ptr & taskSynchronizer) (/home/fresh/data/work/intel_openvino/source/dldt-2019_R1.0.1/inference-engine/src/inference_engine/cpp_interfaces/ie_task.cpp:48)
libclDNNPlugin.so!InferenceEngine::AsyncInferRequestThreadSafeDefault::Infer_ThreadUnsafe(InferenceEngine::AsyncInferRequestThreadSafeDefault * const this) (/home/fresh/data/work/intel_openvino/source/dldt-2019_R1.0.1/inference-engine/src/inference_engine/cpp_interfaces/impl/ie_infer_async_request_thread_safe_default.hpp:220)
libclDNNPlugin.so!InferenceEngine::AsyncInferRequestThreadSafeInternal::Infer(InferenceEngine::AsyncInferRequestThreadSafeInternal * const this) (/home/fresh/data/work/intel_openvino/source/dldt-2019_R1.0.1/inference-engine/src/inference_engine/cpp_interfaces/impl/ie_infer_async_request_thread_safe_internal.hpp:71)
libclDNNPlugin.so!InferenceEngine::InferRequestBase<InferenceEngine::AsyncInferRequestThreadSafeDefault>::Infer(InferenceEngine::InferRequestBase<InferenceEngine::AsyncInferRequestThreadSafeDefault> * const this, InferenceEngine::ResponseDesc * resp) (/home/fresh/data/work/intel_openvino/source/dldt-2019_R1.0.1/inference-engine/src/inference_engine/cpp_interfaces/base/ie_infer_async_request_base.hpp:32)
InferenceEngine::InferRequest::Infer(InferenceEngine::InferRequest * const this) (/home/fresh/data/work/intel_openvino/source/dldt-2019_R1.0.1/inference-engine/include/cpp/ie_infer_request.hpp:101)
main(int argc, char ** argv) (/home/fresh/data/work/intel_openvino/source/dldt-2019_R1.0.1/inference-engine/samples/object_detection_sample_ssd/main.cpp:311)
```
