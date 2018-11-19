# 简介
    ArcStreaming是一款支持录制推流美颜功能的工具，用户可以使用自带的ArcCamera生成默认的视频源，也可以创建自己自定义的视频源进行推送。
    ArcStreaming包含录音功能，用户不需要关心麦克风相关，Object-c风格接口，未来会添加swift风格。为了正常采集和录音，需要用户在App的
    plist.info添加权限：Privacy - Camera Usage Description（使用ArcCamera需要摄像头权限）和Privacy - Microphone Usage Description（麦克风权限）

## 安装方式
    在podfile文件里面添加ArcStreaming即可，如下：
    
    target 'SampleTarget' do
        pod 'ArcStreaming'
    end

## 环境依赖
    要求iOS8.0以上（包括）
    
## 支持输出分辨率
    不关心源的video分辨率，根据用户设置的输出分辨率进行处理。支持输出分辨率如下：（后续根据需要添加），fps帧率需要在设置该枚举时候同步传入，建议
    fps：20~25之间，如果有特殊分辨率或者对音频采样有要求，可以通过- (void) setVideoInfo:(LPSTREAMVIDEOINFO) videoinfo 
    audioInfo:(LPSTREAMAUDIOINFO) audioInfo来设置。
    注意：设置ArcVideoProfile会对源进行裁剪（不关心源宽高比），而setVideoInfo不会对源裁剪，需要保证源和设置的dwPicWidth宽dwPicHeight高比一致才
    不会产生拉伸问题，如源720x1280，设置的dwPicWidth宽dwPicHeight高为360x640，由于宽高比都是9:16，所以不会产生拉伸问题
    
   | 输出分辨率  | 帧率    |  枚举值  |
   | --------   | -----:   | :----: |
   | 360x640        | fps      |   ArcVideoProfile_Portrait_360P    |
   | 450x800        | fps      |   ArcVideoProfile_Portrait_450P    |
   | 480x854        | fps      |   ArcVideoProfile_Portrait_480P    |
   | 720x1280        | fps      |   ArcVideoProfile_Portrait_720P    |
   | 1080x1920        | fps      |   ArcVideoProfile_Portrait_1080P   |
   | 640x360        | fps      |   ArcVideoProfile_Landscape_360P   |
   | 800x450        | fps      |   ArcVideoProfile_Landscape_450P   |
   | 854x480        | fps      |   ArcVideoProfile_Landscape_480P   |
   | 1280x720        | fps      |   ArcVideoProfile_Landscape_720P   |
   | 1920x1080        | fps      |   ArcVideoProfile_Landscape_1080P  |

## 支持动态码率
    推流过程中，网络波动是很正常的，为了保证推流的稳定性，SDK提供了动态码率调整功能。minBitrate：调整码率的下限，maxBitrate：码率调整的上限。
    码率的下限表示无论网络多差，需要保证最低画质质量传输，码率的上限表示无论网络多好，传输的画质保证稳定即可。建议：minBitrate为宽*高/2，
    maxBitrate为宽*高*2
    ···
    - (ArcStreamErrorType)setMinBitrate:(NSUInteger)minBitrate maxBitrate:(NSUInteger)maxBitrate
    ···
