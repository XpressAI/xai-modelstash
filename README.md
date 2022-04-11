# XAI Modelstash Components

XAI Library for xircuits - modelstash integration.

## To Setup for Modelstash
1. Ensure you have modelstash up and running, ie running on localhost. Feel free to ping me if you need help.
2. pip install the modelstash wheel. Again, ping me for the .whl file

## New Components

- `StartModelStashSession` - you will need this component to authenticate the connection with modelstash. Alternatively I have also provided a way for auto authentication by uncommenting the lines after the imports. (ping me for the username and password)
- `ListModels` & `ListSkills`  - list models or skills using modelstash-cli
- `UploadToModelStash` - uploads the file specified by `model_file_path`. You can actually upload anything. `model_name`, `training_dataset_name`, `input_names`, and `output_names` will be auto-inferred from the .xpipes filename if not provided and `creator_name` from `os.getlogin()`
- `DeleteModelfromModelStash` - a component that goes against the ms owner's design principals, but I made it anyways so it would be easy to delete models.
- `DownloadLinkfromModelStash` - generate a download link to download a model from model stash
- `LoadfromModelStash` - loads a model from modelstash given the model file name. Will throw a message if model is not found. If found, downloads it from modelstash, unzips it in current dir, and loads it using keras.model. Returns a model.

## To Test

I've provided 2 examples for the modelstash demo. These demos were tested using the weather image dataset: https://www.kaggle.com/vijaygiitk/multiclass-weather-dataset. 

I've removed the csv file and the alien folder, and placed it in Datasets so that the dataset dir looks like this:

```
xpipes\Datasets\weather_dataset\cloudy
xpipes\Datasets\weather_dataset\foggy
xpipes\Datasets\weather_dataset\rainy
xpipes\Datasets\weather_dataset\shine
xpipes\Datasets\weather_dataset\sunrise
```

### 1. MSTrainUpload

![image](https://user-images.githubusercontent.com/68586800/146880967-86f3195b-1e08-46d1-9f05-cc879f78474d.png)

This example extends the core train image classifier model with SaveKerasModel component, to which you can then connect with the modelstash components `StartModelStashSession` and `UploadToModelStash`. It should automatically upload the model, provided that the modelstash does not already have a model existing with the same name. If so, you can use the `DeleteModelfromModelStash` component to delete it. 

#### Expected Output:
```
Executing: StartModelStashSession

Executing: UploadToModelStash
model_file_path='examples/MSTrainUpload.h5'
model_name='MSTrainUpload'
creator_name='Fahreza'
training_dataset_name='Datasets/weather_dataset'
input_names=['MSTrainUpload_input']
output_names=['MSTrainUpload_output']
examples/MSTrainUpload.h5: 100%|███████████████████████████████████████████████████████| 14.3M/14.3M [00:01<00:00, 10.6MB/s]
```


### 2. MSLoadModel

![image](https://user-images.githubusercontent.com/68586800/146881071-6e10572c-e8e8-4496-b89d-247baa7efba3.png)

This example extends the core Keras predict xircuits. Instead of loading a premade Keras model, you can load from modelstash and directly perform inference on it. 

#### Expected Output:

```
Executing: StartModelStashSession

Executing: LoadfromModelStash
MSTrainUpload_Initial.zip: 13.5MiB [00:01, 8.40MiB/s]

Executing: KerasPredict
Keras model sequential config not found!

Auto adjusting according to model input.

[[0.11990469 0.09423562 0.03890683 0.04541461 0.70153826]]
```
