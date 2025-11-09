---
icon: fontawesome/solid/hippo
---

Before creating an elastic training container cluster, ensure the following information. If relevant operations need to be taken, please consult your administrator. For relevant configurations, please refer to the relevant sections in the Administrator Manual.

### Before You Begin

1. Ensure that your project has access to available MLS Spec

    - The specification must **not** include shared GPU mode
    - Confirm the specification is available for your project

2. Ensure that the Public Image List provides suitable images

    - Images compatible with DeepSpeed and Horovod frameworks are required
    - If no suitable images are available, you can first use the container service to create a container, install the training framework yourself, and then create a custom image. Select the custom image with the training framework installed when creating the cluster.


!!! warning "Notice"

    1. Containers created in the same batch must maintain consistent resource specifications. Different batches can be configured with different specifications, but it is recommended to maintain consistent specifications within the same cluster to ensure optimal performance and stability.

    2. GPU resources are allocated in whole units (per GPU) for elastic training. Deploying training containers on nodes with shared GPU configurations is not supported.

## Create Container Clusters

1. Access the Create Cluster Page
    - Go to 【Container Management】>【Elastic Training Cluster】
    - Click the ++"Create a Cluster"++ button

2. **Basic Settings**
    - Select **Cluster Type** to create. Horovod and DeepSpeed frameworks are currently supported.
    - Enter **Cluster Name** . 
    - Enter **Description** (Optional).
    - Set **Schedule Time** ：
        - **Start Time** : The default setting is Execute Immediately. You can also specify the date and time.
        - **End Time** : The default setting is N/A. You can also specify the date and time or running hours.
    - Click [Next] to configure Resources

    ![Basic Settings](../../img/ethpc/01建立容器叢集-基本設定EN.png)

3. **Resources**
    - Set **Container Count** : The number must be greater than or equal to 2, which includes a launcher container and one or more worker containers.
        - Launcher container: Responsible for unified configuration of container startup parameters, executing launch commands and managing communication flow
        - Worker container: Handles the actual training tasks


    - Select an **Image** with the training framework based on the cluster type.
    - Configure the service **password** (e.g., SSH)
    - **Enable GPU** (active by default)
    - Select **Specification** based on your training needs. If you choose an Nvidia GPU, select CUDA Version

        !!! note

            After selecting a Specification, the Quota section at the top will display the quota usage of the selected specification. You can click to expand and view detailed GPU, vCPU, and RAM usage information, including current usage, quota limits, and remaining quota.

            ![額度](../../img/ethpc/04建立容器叢集-額度EN.png){:height="40%" width="40%"}



    - Set Shared Memory (active by default, recommended to set a minimum capacity of 1 GB)
    - Click [Next] to configure Storage

    ![Resources](../../img/ethpc/02建立容器叢集-資源配置EN.png)

4. **Storage**
    - Home storage:
        - Configure mounting of **home storage** device, select the device to mount. If needed, you can enable [Default as Jupyter Workspace]

        - If a new storage device needs to be added, click the [+] button on the right or [Create Storage] button


        !!! note "Home Storage"
             The platform mounts users' Home directories into the training environment. After mounting, files such as data generated during training, model cache, and personalized settings will be stored in this directory. When you need to find training-related data, you can search in the Home directory.
        
            !!! warning ""

                1. Do not store large amounts of temporary files that don't have to be retained in the Home directory.
                2. Files under this directory will not be automatically deleted when a training cluster is removed.

    - Internal storage:

        - If needed, enable mounting of **internal storage** .

        - Select the internal storage device to mount and configure the mounting path.
    
    ![Storage](../../img/ethpc/06建立容器叢集-掛載home儲存EN.png)
    
5. **Summary**
    - Confirm all configurations. If you need to modify configurations, click the edit icon in the upper right corner to go back and edit.

    - Click the [Submit] button

6. Eeturn to the 【Elastic Training Cluster】 page after submitting.

    - When the status of the container cluster changes from `Creating` to `Created`, the elastic training cluster has been created successfully.

    ![List](../../img/ethpc/09建立容器叢集-列表建立中EN.png)