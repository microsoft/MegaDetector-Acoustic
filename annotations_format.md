# Annotation Format

The lack of standardized annotation formats in bioacoustics has made collaboration between research teams and data integration challenging, limiting the potential for large-scale studies and the development of effective machine learning models. We aim to establish a unified format for annotating biacoustic data, inspired by the widely used COCO format in computer vision. 

## Standardized format

The proposed JSON format contains the following sections:

```json
{
    "info": {
        "title": "Dataset Title",
        "license": "CC BY 4.0",
        "publication_date": "20250101",
        "description": "This is a brief summary of the dataset",
        "creators": [
            {
                "name": "John Doe",
                "affiliation": "Organization Name"
            }
        ],
        "version": 1.0,
        "url": "https://example.com"
    },
    "categories": [
        {
            "id": 0,
            "name": "Melanerpes formicivorus"
        }
    ],
    "sounds": [
        {
            "id": 0,
            "file_name_path": "datasets/audios/recording1.wav",
            "duration": 120.5,
            "sample_rate": 48000,
            "latitude": 10.11,
            "longitude": -84.52,
            "date_recorded": "20230915"
        }
    ],
    "annotations": [
        {
            "anno_id": 0,
            "sound_id": 0,
            "category_id": 0,
            "category": "Melanerpes formicivorus",
            "supercategory": "animal",
            "t_min": 0.0,
            "t_max": 10.0,
            "f_min": 300.0,
            "f_max": 8000.0,
            "ismultilabel": false
        }
    ]
}  
```
As evidenced in the example above, our format consists of four main parts: 
- `info`: General information about the dataset
- `categories`: List of categories in the dataset
- `sounds`: List of sound files in the dataset
- `annotations`: List of annotations for the sound files 
  
Now, let's take a detailed look at the information that comprises these sections.

## Info
  
| Name           | Definition                                                                                     | Type   |  
|----------------|------------------------------------------------------------------------------------------------|--------|  
| `title` | The title of the bioacoustic dataset. <br><br> **Constraints** <br>• required: `true` | `str` |  
| `license`         | The name of the license that specifies the permissions and restrictions for using the bioacoustic dataset. <br><br> **Constraints** <br>• required: `true` <br><br> **Example:** `CC BY 4.0` <br><br> | `str`      | 
| `publication_date` | The datetime when the bioacoustic dataset was published in YYYY-MM-DD format. <br><br> **Constraints** <br>• required: `false` | `datetime` | 
| `description`  | A brief summary of the dataset. <br><br> **Constraints** <br>• required: `false` | `str`      |  
| `creators`  | List with creators information. <br><br> **Constraints** <br>• required: `false` | `str`      |  
| `version`      | The version number of the dataset. <br><br> **Constraints** <br>• required: `false` | `float`      |  
| `url`          | The web address where the dataset can be accessed or downloaded.  If it's a Zenodo URL, metadata is fetched automatically. <br><br> **Constraints** <br>• required: `false` | `str`      |  

## Categories 
  
| Name           | Definition                                                                                     | Type   |  
|----------------|------------------------------------------------------------------------------------------------|--------|
| `id`           | A unique identifier for a category within the dataset. <br><br> **Constraints** <br>• required: `true` <br><br> **Example:** `0`| `int`      |
| `name`    | The name of the category of sounds, such as a particular species or type of call. <br><br> **Constraints** <br>• required: `true` <br><br> **Example:** `Melanerpes formicivorus` | `str`      |  

This section is built from a DataFrame that must include at least the category names and their identifiers. However, it can also contain other relevant category information, such as eBird Codes, Common Names, etc.

## Sounds 
  
| Name           | Definition                                                                                     | Type   |  
|----------------|------------------------------------------------------------------------------------------------|--------|  
| `id`           | A unique identifier for a specific sound within the dataset. <br><br> **Constraints** <br>• required: `true` <br><br> **Example:** `1`| `int`      |  
| `file_name_path`    | The path of the audio file containing the bioacoustic recording. <br><br> **Constraints** <br>• required: `true` <br><br> **Example:** `datasets/audios/recording1.wav` | `str`      |  
| `duration`     | The length of the audio recording in seconds. <br><br> **Constraints** <br>• required: `true` <br><br> **Example:** `120.5`| `float`      |  
| `sample_rate`  | The number of samples of audio carried per second, measured in Hz. <br><br> **Constraints** <br>• required: `true` <br><br> **Example:** `48000`| `int`      |  
| `latitude`     | The geographical latitude where the bioacoustic recording was taken. <br><br> **Constraints** <br>• required: `true` <br><br> **Example:** `10.11` | `float`    |  
| `longitude`    | The geographical longitude where the bioacoustic recording was taken. <br><br> **Constraints** <br>• required: `true` <br><br> **Example:** `-84.52` | `float`    |  
| `date_recorded`| The datetime when the audio was recoded in YYYY-MM-DD format. <br><br> **Constraints** <br>• required: `false` | `datetime`    |  


## Annotations 
  
| Name           | Definition                                                                                     | Type   |  
|----------------|------------------------------------------------------------------------------------------------|--------|  
| `anno_id`           | A unique identifier for a specific annotation within the dataset. <br><br> **Constraints** <br>• required: `true` <br><br> **Example:** `1` | `int`      |  
| `sound_id`     | The identifier of the sound to which the annotation is linked. <br><br> **Constraints** <br>• required: `true` <br><br> **Example:** `1` | `int`      |  
| `category_id`  | The identifier of the category to which the sound belongs. <br><br> **Constraints** <br>• required: `true` <br><br> **Example:** `0` | `int`      | 
| `category`  | The name of the category of sounds, such as a particular species or type of call. <br><br> **Constraints** <br>• required: `true` <br><br> **Example:** `"Melanerpes formicivorus"` | `int`      | 
| `supercategory`| A higher-level grouping that the category belongs to. <br><br> **Constraints** <br>• required: `false` | `str`      |   
| `t_min`        | The starting time of the annotated sound within the recording, in seconds. <br><br> **Constraints** <br>• required: `true` <br><br> **Example:** `0.0` | `float`    |  
| `t_max`        | The ending time of the annotated sound within the recording, in seconds. <br><br> **Constraints** <br>• required: `true` <br><br> **Example:** `10.0` | `float`    |  
| `f_min`        | The lowest frequency of the annotated sound within the recording, in Hz. <br><br> **Constraints** <br>• required: `false`| `float`    |  
| `f_max`        | The highest frequency of the annotated sound within the recording, in Hz. <br><br> **Constraints** <br>• required: `false` | `float`    |  
| `ismultilabel`     |  A boolean indicating whether the sound is labeled with multiple classes simultaneously. <br><br> **Constraints** <br>• required: `true` <br><br> **Example:** `False` <br><br> | `bool`     |