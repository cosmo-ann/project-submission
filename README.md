{
  "nbformat": 4,
  "nbformat_minor": 0,
  "metadata": {
    "colab": {
      "name": "python and cnn shapeai.ipynb",
      "provenance": [],
      "collapsed_sections": []
    },
    "kernelspec": {
      "name": "python3",
      "display_name": "Python 3"
    },
    "language_info": {
      "name": "python"
    }
  },
  "cells": [
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "FGr7HdQQ97cz",
        "outputId": "7b8317b1-1e9c-44ef-d00d-2c360cb40f6f"
      },
      "source": [
        "!pip install keras-tuner"
      ],
      "execution_count": 1,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "Collecting keras-tuner\n",
            "\u001b[?25l  Downloading https://files.pythonhosted.org/packages/20/ec/1ef246787174b1e2bb591c95f29d3c1310070cad877824f907faba3dade9/keras-tuner-1.0.2.tar.gz (62kB)\n",
            "\r\u001b[K     |█████▏                          | 10kB 12.6MB/s eta 0:00:01\r\u001b[K     |██████████▍                     | 20kB 16.1MB/s eta 0:00:01\r\u001b[K     |███████████████▋                | 30kB 20.1MB/s eta 0:00:01\r\u001b[K     |████████████████████▉           | 40kB 23.5MB/s eta 0:00:01\r\u001b[K     |██████████████████████████      | 51kB 25.9MB/s eta 0:00:01\r\u001b[K     |███████████████████████████████▎| 61kB 28.3MB/s eta 0:00:01\r\u001b[K     |████████████████████████████████| 71kB 7.0MB/s \n",
            "\u001b[?25hRequirement already satisfied: packaging in /usr/local/lib/python3.7/dist-packages (from keras-tuner) (20.9)\n",
            "Requirement already satisfied: future in /usr/local/lib/python3.7/dist-packages (from keras-tuner) (0.16.0)\n",
            "Requirement already satisfied: numpy in /usr/local/lib/python3.7/dist-packages (from keras-tuner) (1.19.5)\n",
            "Requirement already satisfied: tabulate in /usr/local/lib/python3.7/dist-packages (from keras-tuner) (0.8.9)\n",
            "Collecting terminaltables\n",
            "  Downloading https://files.pythonhosted.org/packages/9b/c4/4a21174f32f8a7e1104798c445dacdc1d4df86f2f26722767034e4de4bff/terminaltables-3.1.0.tar.gz\n",
            "Collecting colorama\n",
            "  Downloading https://files.pythonhosted.org/packages/44/98/5b86278fbbf250d239ae0ecb724f8572af1c91f4a11edf4d36a206189440/colorama-0.4.4-py2.py3-none-any.whl\n",
            "Requirement already satisfied: tqdm in /usr/local/lib/python3.7/dist-packages (from keras-tuner) (4.41.1)\n",
            "Requirement already satisfied: requests in /usr/local/lib/python3.7/dist-packages (from keras-tuner) (2.23.0)\n",
            "Requirement already satisfied: scipy in /usr/local/lib/python3.7/dist-packages (from keras-tuner) (1.4.1)\n",
            "Requirement already satisfied: scikit-learn in /usr/local/lib/python3.7/dist-packages (from keras-tuner) (0.22.2.post1)\n",
            "Requirement already satisfied: pyparsing>=2.0.2 in /usr/local/lib/python3.7/dist-packages (from packaging->keras-tuner) (2.4.7)\n",
            "Requirement already satisfied: urllib3!=1.25.0,!=1.25.1,<1.26,>=1.21.1 in /usr/local/lib/python3.7/dist-packages (from requests->keras-tuner) (1.24.3)\n",
            "Requirement already satisfied: idna<3,>=2.5 in /usr/local/lib/python3.7/dist-packages (from requests->keras-tuner) (2.10)\n",
            "Requirement already satisfied: chardet<4,>=3.0.2 in /usr/local/lib/python3.7/dist-packages (from requests->keras-tuner) (3.0.4)\n",
            "Requirement already satisfied: certifi>=2017.4.17 in /usr/local/lib/python3.7/dist-packages (from requests->keras-tuner) (2020.12.5)\n",
            "Requirement already satisfied: joblib>=0.11 in /usr/local/lib/python3.7/dist-packages (from scikit-learn->keras-tuner) (1.0.1)\n",
            "Building wheels for collected packages: keras-tuner, terminaltables\n",
            "  Building wheel for keras-tuner (setup.py) ... \u001b[?25l\u001b[?25hdone\n",
            "  Created wheel for keras-tuner: filename=keras_tuner-1.0.2-cp37-none-any.whl size=78938 sha256=d44980171d205dcaca28b12006e13d4d53ee936b3bce93c1f88e8ee8281959bf\n",
            "  Stored in directory: /root/.cache/pip/wheels/bb/a1/8a/7c3de0efb3707a1701b36ebbfdbc4e67aedf6d4943a1f463d6\n",
            "  Building wheel for terminaltables (setup.py) ... \u001b[?25l\u001b[?25hdone\n",
            "  Created wheel for terminaltables: filename=terminaltables-3.1.0-cp37-none-any.whl size=15356 sha256=8e8154ef8a74fdfa71f90002f3daeb29538244b190acbfe879af46fa390d6803\n",
            "  Stored in directory: /root/.cache/pip/wheels/30/6b/50/6c75775b681fb36cdfac7f19799888ef9d8813aff9e379663e\n",
            "Successfully built keras-tuner terminaltables\n",
            "Installing collected packages: terminaltables, colorama, keras-tuner\n",
            "Successfully installed colorama-0.4.4 keras-tuner-1.0.2 terminaltables-3.1.0\n"
          ],
          "name": "stdout"
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "BM00vbdW-l9I"
      },
      "source": [
        "import tensorflow as tf\n",
        "from tensorflow import keras\n",
        "import numpy as np"
      ],
      "execution_count": 3,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "w0VQjIaE-7Hw"
      },
      "source": [
        "fashion_mnist=keras.datasets.fashion_mnist"
      ],
      "execution_count": 4,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "CqSv-NRs_K6z",
        "outputId": "55c6e96f-09c1-43c2-cdc8-6f178d637a4d"
      },
      "source": [
        "(train_images , train_labels),(test_images , test_labels)=fashion_mnist.load_data()"
      ],
      "execution_count": 5,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "Downloading data from https://storage.googleapis.com/tensorflow/tf-keras-datasets/train-labels-idx1-ubyte.gz\n",
            "32768/29515 [=================================] - 0s 0us/step\n",
            "Downloading data from https://storage.googleapis.com/tensorflow/tf-keras-datasets/train-images-idx3-ubyte.gz\n",
            "26427392/26421880 [==============================] - 0s 0us/step\n",
            "Downloading data from https://storage.googleapis.com/tensorflow/tf-keras-datasets/t10k-labels-idx1-ubyte.gz\n",
            "8192/5148 [===============================================] - 0s 0us/step\n",
            "Downloading data from https://storage.googleapis.com/tensorflow/tf-keras-datasets/t10k-images-idx3-ubyte.gz\n",
            "4423680/4422102 [==============================] - 0s 0us/step\n"
          ],
          "name": "stdout"
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "mLNV9fKY_jD4"
      },
      "source": [
        "train_images = train_images/255.0\n",
        "test_images = test_images/255.0"
      ],
      "execution_count": 6,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "Rdo5TTsu_12Y",
        "outputId": "67e42e85-61ad-4158-ae33-2c69530c4309"
      },
      "source": [
        "train_images[0].shape"
      ],
      "execution_count": 7,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "(28, 28)"
            ]
          },
          "metadata": {
            "tags": []
          },
          "execution_count": 7
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "NzYXsiAw_9qt"
      },
      "source": [
        "train_images = train_images.reshape(len(train_images),28,28,1)\n",
        "test_images = test_images.reshape(len(test_images),28,28,1)"
      ],
      "execution_count": 8,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "05qRE7TrAX9P"
      },
      "source": [
        "def build_model(hp):\n",
        "  model = keras.Sequential([\n",
        "    keras.layers.Conv2D(\n",
        "        filters = hp.Int('conv_1_filter', min_value = 32, max_value = 128, step=16),\n",
        "        kernel_size= hp.Choice('conv_1_kernel', values= [3,5]),\n",
        "        activation= 'relu',\n",
        "        input_shape=(28,28,1)\n",
        "    ),\n",
        "    keras.layers.Conv2D(\n",
        "        filters = hp.Int('conv_2_filter', min_value = 32, max_value = 64, step=16),\n",
        "        kernel_size= hp.Choice('conv_2_kernel', values= [3,5]),\n",
        "        activation= 'relu'\n",
        "                                \n",
        "    ),\n",
        "    keras.layers.Flatten(),\n",
        "    keras.layers.Dense(\n",
        "        units = hp.Int('dense_1_units' , min_value = 32, max_value=128, step= 16),\n",
        "        activation= 'relu'\n",
        "    ),\n",
        "    keras.layers.Dense(10, activation = 'softmax')\n",
        "  ])\n",
        "\n",
        "  model.compile(optimizer=keras.optimizers.Adam(hp.Choice('learning_rate',values=[1e-2, 1e-3])),\n",
        "                loss='sparse_categorical_crossentropy',\n",
        "                metrics=['accuracy'])\n",
        "                \n",
        "            \n",
        "\n",
        "  return model\n",
        "\n",
        "\n",
        "                                                \n",
        "                                                \n",
        "                                \n",
        "                                \n",
        "                            \n",
        "  "
      ],
      "execution_count": 29,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "B-uSgfG1H9SH"
      },
      "source": [
        "from kerastuner import RandomSearch\n",
        "from kerastuner.engine.hyperparameters import HyperParameters"
      ],
      "execution_count": 26,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "Tpi9gc2fIkeN",
        "outputId": "2b4c6f14-790a-4008-f5a2-a07f3f4c9bbd"
      },
      "source": [
        "tuner_search=RandomSearch(build_model,\n",
        "                          objective='val_accuracy',\n",
        "                          max_trials=5,directory='output',project_name='Mnist Fashion')"
      ],
      "execution_count": 35,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "INFO:tensorflow:Reloading Oracle from existing project output/Mnist Fashion/oracle.json\n"
          ],
          "name": "stdout"
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "BHoTJJf9Lbn0",
        "outputId": "49faf85f-d290-4b2a-e1b2-7cb75162ea31"
      },
      "source": [
        "tuner_search.search(train_images,train_labels,epochs=3,validation_split=0.1)"
      ],
      "execution_count": 36,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "Trial 5 Complete [00h 18m 05s]\n",
            "val_accuracy: 0.8669999837875366\n",
            "\n",
            "Best val_accuracy So Far: 0.9169999957084656\n",
            "Total elapsed time: 01h 36m 47s\n",
            "INFO:tensorflow:Oracle triggered exit\n"
          ],
          "name": "stdout"
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "7JYiqNBBgGUZ"
      },
      "source": [
        "model=tuner_search.get_best_models(num_models=1)[0]"
      ],
      "execution_count": 37,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "qK287jmXge1i",
        "outputId": "0dc6f64e-f255-4dc2-e896-eeafead3dd86"
      },
      "source": [
        "model.summary()"
      ],
      "execution_count": 39,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "Model: \"sequential\"\n",
            "_________________________________________________________________\n",
            "Layer (type)                 Output Shape              Param #   \n",
            "=================================================================\n",
            "conv2d (Conv2D)              (None, 26, 26, 96)        960       \n",
            "_________________________________________________________________\n",
            "conv2d_1 (Conv2D)            (None, 22, 22, 32)        76832     \n",
            "_________________________________________________________________\n",
            "flatten (Flatten)            (None, 15488)             0         \n",
            "_________________________________________________________________\n",
            "dense (Dense)                (None, 112)               1734768   \n",
            "_________________________________________________________________\n",
            "dense_1 (Dense)              (None, 10)                1130      \n",
            "=================================================================\n",
            "Total params: 1,813,690\n",
            "Trainable params: 1,813,690\n",
            "Non-trainable params: 0\n",
            "_________________________________________________________________\n"
          ],
          "name": "stdout"
        }
      ]
    }
  ]
}
