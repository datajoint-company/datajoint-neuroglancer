## Neuroglancer Utilities

The primary feature provided here is a routine to convert images to **precomputed** format for use with **Neuroglancer**. 
This routine includes three primary steps:

1. Download image stack from S3 bucket, guided by an ordered filenames text file
2. Convert to **precomputed** format
3. Re-upload the **precomputed** to S3 bucket

This routine can be executed using python script as followed:
```
python neuroglancer-utilities s3_convert_to_precomputed_config.json
```
where **s3_convert_to_precomputed_config.json** is a JSON config file specifying all the necesssary parameters.
The JSON config file is structured with the following fields:

+	*"s3_creds_file"*: "./s3-creds.json"  # AWS credentials JSON config file (required fields: **access_key**, **secret_key**)
+	*"s3_bucket_name_for_download"*: "mousebrainatlas-data"  # name of the S3 bucket, that the data is in
+	*"sorted_filename"*: "MD585_sorted_filenames.txt"  # the sorted filename list to specify the sequential order of image slices
+	*"ext"*: ".jpg"  # image file extension (e.g. .jpg, .tif)
+	*"s3_prefix"*: "CSHL_data_processed/MD585/MD585_prep2_down8_grayJpeg/"  #  path prefix to specify where the data is on this S3 bucket
+	*"s3_parts"*: "_prep2_down8_grayJpeg"  # other string filter to identify the file on this S3 bucket
+	*"folder_to_write_to"*: "./temp_s3_download/MD585_prep2_down8_grayJpeg"  # local/temporary folder to download the data to
+	*"folder_to_convert_from"*: "./temp_s3_download/MD585_prep2_down8_grayJpeg"  # folder containing images data to be converted to **precomputed** format, typically the same as *"folder_to_write_to"* 
+	*"folder_to_convert_to"*: "./temp_s3_download/MD585_prep2_down8_grayJpeg_precomputed"
+	*"s3_bucket_name_for_upload"*: "mousebrainatlas-datajoint-jp2k"  # S3 bucket name to upload the **precomputed** data to
+	*"s3_dir_to_write_to"*: "precomputed/MD585_down8_grayJpeg"  # S3 path prefix on this bucket to upload the **precomputed** data to
+	*"voxel_resolution"*: [3680, 3680, 20000]  # voxel resolution of the image stack
+	*"voxel_offset"*: [0, 0, 0]  # voxel offset of the image stack
+	*"overwrite"*: false  # overwrite replicated filename when uploading to S3 bucket

This module also provides tools for implementing each of the above three steps individually. 

Note: the conversion routine here assumes that each image file represent a single 2D slice of a larger 3D volume, with all slices having the same dimension. 
This implies certain preprocessing steps to be done on the images prior to the conversion (e.g. realignment, crop, etc.).



