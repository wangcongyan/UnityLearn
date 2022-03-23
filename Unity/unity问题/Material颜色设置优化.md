## urp下fairygui遮挡场景显示:
    Camera camera = FairyGUI.StageCamera.main;
    Camera mainCamera = Camera.main;
    UniversalAdditionalCameraData data = camera.GetUniversalAdditionalCameraData();
    data.renderType = CameraRenderType.Overlay;
    mainCamera.GetUniversalAdditionalCameraData().cameraStack.Add(camera);
    UIConfig.modalLayerColor = new Color(0f, 0f, 0f, 0.8f);


## MaterialPropertyBlock 优化
    ![Image text](/unity问题/Image/20180822084523853.jpg)

//会额外生成一个材质实例造成内存浪费
    void CCChangeColor()
    {
        Transform[] ts = gameObject.GetComponentsInChildren<Transform>();
        objects = new GameObject[ts.Length];
        for (int i = 0; i < ts.Length; i++) 
        {
            objects[i] = ts[i].gameObject;
        }
        MeshRenderer renderer;
        foreach (GameObject obj in objects) 
        {
            float r = Random.Range(0.0f, 1.0f);
            float g = Random.Range(0.0f, 1.0f);
            float b = Random.Range(0.0f, 1.0f);
            renderer = obj.GetComponent<MeshRenderer>();
            if (renderer == null) 
            {
                continue;
            }
            renderer.material.color = new Color(r, g, b);
        }
    }

    //通过MaterialPropertyBlock设置颜色修改meshrender颜色
    void ChangeColorByProperty()
    {

        Transform[] ts = gameObject.GetComponentsInChildren<Transform>();
        objects = new GameObject[ts.Length];
        for (int i = 0; i < ts.Length; i++)
        {
            objects[i] = ts[i].gameObject;
        }
        MeshRenderer renderer;
        MaterialPropertyBlock block = new MaterialPropertyBlock();
        foreach (GameObject obj in objects)
        {
            float r = Random.Range(0.0f, 1.0f);
            float g = Random.Range(0.0f, 1.0f);
            float b = Random.Range(0.0f, 1.0f);
            renderer = obj.GetComponent<MeshRenderer>();
            block.SetColor("_Color", new Color(r, g, b));
            if (renderer == null) {
                continue;
            }
            renderer.SetPropertyBlock(block);
        }
    }