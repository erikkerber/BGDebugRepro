load("@build_bazel_rules_apple//apple:apple.bzl", "local_provisioning_profile")
load("@build_bazel_rules_apple//apple:ios.bzl", "ios_application", "ios_build_test",)
load("@build_bazel_rules_apple//apple:resources.bzl", "apple_resource_group")
load("@build_bazel_rules_swift//swift:swift.bzl", "swift_library")
load(
    "@build_bazel_rules_apple//apple:resources.bzl",
    "apple_core_data_model",
)
load("@rules_xcodeproj//xcodeproj:defs.bzl", "xcode_provisioning_profile")


filegroup(
    name = "swift_datamodel",
    srcs = glob(["ColorFeed/ColorFeed.xcdatamodeld/**"]),
)

apple_core_data_model(
    name = "datamodel",
    srcs = [":swift_datamodel"],
)

swift_library(
    name = "ColorsLib",
    srcs = glob(["ColorFeed/**/*.swift"]) + ["datamodel"],
    data = glob(["ColorFeed/ColorFeed.xcdatamodeld/**"]),
    module_name = "ColorsLib",
    deps = [
    ],
)

ios_build_test(
    name = "Colors.ios",
    minimum_os_version = "15.0",
    targets = [
        ":ColorsLib",
    ],
)

config_setting(
    name = "device_build",
    values = {
        "cpu": "ios_arm64",
    },
)

xcode_provisioning_profile(
    name = "xcode_profile",
    managed_by_xcode = True,
    provisioning_profile = ":xcode_managed_profile",
    tags = ["manual"],
)

local_provisioning_profile(
    name = "xcode_managed_profile",
    profile_name = "iOS Team Provisioning Profile: com.foo.bar",
    tags = ["manual"],
    team_id = "VA25F5RN8B",
)

ios_application(
    name = "Colors",
    bundle_id = "com.slackdev.Colors",
    bundle_name = "Colors",
    # entitlements = "ios app.entitlements",
    # executable_name = "iOSApp_ExecutableName",
    families = ["iphone"],
    # frameworks = [
    #     "//iOSApp/Source/CoreUtilsObjC:FrameworkCoreUtilsObjC",
    #     "//UI:UIFramework.iOS",
    # ],
    infoplists = ["ColorFeed/Info.plist"],
    # launch_images = select({
    #     ":device_build": glob(["launch_images_ios.xcassets/**"]),
    #     "//conditions:default": [],
    # }),
    # launch_storyboard = select({
    #     ":device_build": None,
    #     "//conditions:default": "Launch.storyboard",
    # }),
    minimum_os_version = "15.0",
    provisioning_profile = select({
        ":device_build": ":xcode_profile",
        "//conditions:default": None,
    }),
    resources = glob(
        [
            "ColorFeed/ColorFeed.xcdatamodeld/**",
        ],
    ),
    # version = "//iOSApp:Version",
    visibility = ["//visibility:public"],
    deps = [
        ":Colors.library",
    ],
)
