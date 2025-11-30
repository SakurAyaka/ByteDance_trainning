# ByteDance_trainning_zyx
这是字节跳动训练营的项目，选择任务二，一个Android应用demo。
一、项目概述
  zyx 是一个轻量级 Android 应用，旨在帮助用户快速创建并发布带有文字、图片、位置信息的内容。目前已经开发出一下功能：支持以下核心功能：多图上传与封面设置、拖拽排序图片、支持@用户和#话题的文本输入、自动获取地理位置、本地预览与发布模拟、内容字数限制，预留了后端上传接口，并采用 Kotlin + ViewBinding + RecyclerView + Retrofit + Glide 等现代 Android 开发技术栈，结构清晰，便于维护和二次开发。
二、技术架构
  语言：Kotlin
  UI 构建：XML 布局 + ViewBinding
  网络请求：Retrofit + OkHttp + Gson
  图片加载：Glide
  权限管理：AndroidX 权限兼容库
  定位服务：Google Play Services FusedLocationProviderClient  
三、核心功能
  图片管理：通过系统图库选择单张图片（支持多次添加）进行上传，取第一张作为预览图，点击图片进行预览，长按图片可以拖动图片以完成与其他图片的顺序调整，点击右上角垃圾桶标可删除文件。图片数据模型：ImageItem(id: String, uri: Uri)
  文本编辑：以单行EditText为标题，字数限制最多 500 字并在右下角进行实时字数统计。点击#话题按钮，弹出热门话题弹窗（#旅行、#美食等）热门话题。输入@ 弹出用户选择弹窗（Mock 数据：Alice, Bob...）
  位置信息：在登录时会有使用位置信息的权限请求，用户可选择是否使用位置信息，如果使用，则会在发布界面显示最后已知位置。显示格式：(纬度, 经度)，保留 6 位小数。并支持手动清除位置（点击 × 按钮）。如果不使用，则会显示权限被拒绝，并在下次启动时询问是否更换权限（引导进模拟手机设置中更换）
四、UI/UX 设计
  UI/UX仿照抖音投稿界面，整体风格为深色主题，在交互方面有对应的色块和按钮做交互提醒。在上传图片时，拖拽图片会进行对拖拽方向的轻微位移，如果长按图片，则可以拖动图片与其他图片进行图片顺序调换。所有颜色、尺寸均通过 XML 布局控制，便于主题定制。
五、网络与后端对接（未开发完）
  本次demo中预留了网络接口，当前未在 MainActivity 中调用上传，但已准备好 Retrofit 客户端。预想点击发布后可以跳转相应界面。用户可通过 network_security_config.xml 进行 HTTP ，并开启 HttpLoggingInterceptor，打印完整请求/响应。
  API 接口定义（ApiService.kt）：
  @Multipart
  @POST("upload")
  fun uploadImage(
    @Part("description") description: RequestBody,
    @Part image: MultipartBody.Part
  ): Call<UploadResponse>
  
  响应模型：
  data class UploadResponse(
    val success: Boolean,
    val message: String,
    val data: UploadData? = null
  )
  data class UploadData(
    val imageUrl: String
  )
六、文件结构说明
  1. Java/Kotlin 文件
  MainActivity.kt：项目核心文件，负责初始化控件、处理用户交互（如上传图片、发布内容）、调用系统服务（如位置获取）、管理数据（如图片列表、Mock 用户 / 话题）等。
  ImageAdapter.kt：图片列表的适配器，负责将图片数据绑定到 item_image.xml 布局，处理图片的显示、点击和删除事件（本项目中为 MainActivity 内部类，也可独立成文件）。
  ImageItem.kt：图片数据模型，存储图片的唯一 ID 和 Uri（本项目中为 MainActivity 内部类，也可独立成文件）。
  2. 布局文件
  activity_main.xml：主布局，包含封面图、图片列表、输入框、话题按钮、位置信息、上传图片模块、发布按钮等所有界面元素。
  item_image.xml：图片列表项布局，用于显示单张图片和删除按钮。
  popup_at_users.xml：@用户时弹出的弹窗布局，包含一个列表显示 Mock 用户。
  popup_hot_topics.xml：选择话题时弹出的弹窗布局，包含一个列表显示热门话题。
  3. 资源文件
  colors.xml：定义项目中使用的颜色，如背景色、文字色、按钮色等。
  strings.xml：存储字符串资源（本项目未使用，可将硬编码文字放入此处，便于国际化）。
  styles.xml：定义应用样式（本项目未使用，默认样式即可满足需求）。
  drawable/：存储图片资源，如上传图片的加号图标（本项目用系统图标，可选自定义）。
  mipmap/：存储应用图标，如 launcher 图标（默认生成，无需修改）。
  4. 配置文件
  AndroidManifest.xml：应用清单，声明应用名称、图标、权限（如位置权限、存储权限）、主活动等关键信息。
  build.gradle：模块级构建文件，配置项目依赖（如 Glide、RecyclerView、Google 位置服务）、编译版本、签名信息等。

  




