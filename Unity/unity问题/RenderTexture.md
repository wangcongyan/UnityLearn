//RenderTexture申请内存和销毁内存
RenderTexture.GetTemporary() = > RenderTexture.ReleaseTempporary();
cmd.GetTemporary() = cmd.ReleaseTempporary();
new RenderTexture() = > rt.Release();