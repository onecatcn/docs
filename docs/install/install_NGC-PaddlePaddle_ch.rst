..  _NGC PaddlePaddle install_introduction:

=========
 Paddle NGC 安装指南
=========

-----------
  整体介绍
-----------

PaddlePaddle NGC 容器针对 NVIDIA GPU 加速进行了优化，并包含一组经过验证的库，可启用和优化NVIDIA GPU 性能。此容器还可能包含对 PaddlePaddle 源代码的修改，以最大限度地提高性能和兼容性。此容器还包含用于加速 ETL (DALI, RAPIDS),、训练(cuDNN, NCCL)和推理(TensorRT)工作负载的软件。

-----------
  1.环境准备
-----------

使用 PaddlePaddle NGC 容器需要主机系统安装以下内容：
*	`Docker引擎 <https://docs.docker.com/get-docker/>`_
*	`NVIDIA GPU 驱动程序<https://docs.nvidia.com/datacenter/tesla/tesla-installation-notes/index.html>`_
*	`NVIDIA 容器工具包<https://docs.docker.com/get-docker/>`_
有关支持的版本，请参阅NVIDIA框架容器支持矩阵和 NVIDIA 容器工具包文档。
不需要其他安装、编译或依赖管理。 无需安装 NVIDIA CUDA Toolkit。

-----------
  2.安装步骤
-----------

要运行容器，请按照 NVIDIA Containers For Deep Learning Frameworks User's Guide 中Running A Container一章中的说明发出适当的命令，并指定注册表、存储库和标签。 有关使用 NGC 的更多信息，请参阅 NGC 容器用户指南。
如果您有 Docker 19.03 或更高版本，启动容器的典型命令是：

    ::

        docker run --gpus all -it --rm nvcr.io/nvidia/paddlepaddle:xx.xx-py3


如果您有 Docker 19.02 或更早版本，启动容器的典型命令是：

    ::

        nvidia-docker run -it --rm nvcr.io/nvidia/paddlepaddle:xx.xx-py3



其中:
*	xx.xx 是容器版本。例如，22.06.
PaddlePaddle 通过将其作为 Python 模块导入来运行：

    ::

        $ python -c 'import paddle; paddle.utils.run_check()'
       Running verify PaddlePaddle program ...
       W0516 06:36:54.208734   442 device_context.cc:451] Please NOTE: device: 0, GPU Compute Capability: 8.0, Driver API Version: 11.7, Runtime API Version: 11.7
       W0516 06:36:54.212574   442 device_context.cc:469] device: 0, cuDNN Version: 8.4.
       PaddlePaddle works well on 1 GPU.
       W0516 06:37:12.706600   442 fuse_all_reduce_op_pass.cc:76] Find all_reduce operators: 2. To make the speed faster, some all_reduce ops are fused during training, after fusion, the number of all_reduce ops is 2.
       PaddlePaddle works well on 8 GPUs.
       PaddlePaddle is installed successfully! Let's start deep learning with PaddlePaddle now.

有关入门和自定义 PaddlePaddle 映像的信息，请参阅容器内的 /workspace/README.md。

您可能希望从容器外部的位置提取数据和模型描述以供 PaddlePaddle 使用。 为此，最简单的方法是将一个或多个主机目录挂载为 Docker 绑定挂载。 例如：

    ::

        docker run --gpus all -it --rm -v local_dir:container_dir nvcr.io/nvidia/paddlepaddle:xx.xx-py3


注意：为了在队列之间共享数据，NCCL 可能需要共享系统内存用于 IPC 和固定（页面锁定）系统内存资源。 操作系统对这些资源的限制可能需要相应增加。 有关详细信息，请参阅系统文档。 特别是，Docker 容器默认使用有限的共享和固定内存资源。 在容器内使用 NCCL 时，建议您通过发出以下命令来增加这些资源：

    ::

        --shm-size=1g --ulimit memlock=-1

在 docker run 命令中。

-----------
  3.NGC容器介绍
-----------

有关内容的完整列表，请参阅 PaddlePaddle Container 发行说明。
此容器映像包含 NVIDIA 版 PaddlePaddle 的完整源代码，位于 /opt/paddle/paddle。它是作为系统 Python 模块预构建和安装的。
NVIDIA PaddlePaddle Container 针对与 NVIDIA GPU 一起使用进行了优化，并包含以下用于 GPU 加速的软件：
*	CUDA
*	cuBLAS
*	NVIDIA cuDNN
*	NVIDIA NCCL (optimized for NVLink)
*	NVIDIA Data Loading Library (DALI)
*	TensorRT
*	PaddlePaddle with TensorRT (Paddle-TRT)
此容器中的软件堆栈已经过兼容性验证，不需要最终用户进行任何额外的安装或编译。此容器可以帮助您从端到端加速深度学习工作流程。


-----------
  4.许可协议
-----------

下载、使用NGC PaddlePaddle产品,即表示您同意接受此最终用户许可协议条款和条件之约束。

