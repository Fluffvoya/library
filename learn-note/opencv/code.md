```cpp
#include <iostream>
#include <opencv2/core.hpp>
#include <opencv2/core/hal/interface.h>
#include <opencv2/core/mat.hpp>
#include <opencv2/core/types.hpp>
#include <opencv2/highgui.hpp>
#include <opencv2/imgcodecs.hpp>
#include <opencv2/imgproc.hpp>
#include <opencv2/objdetect.hpp>
#include <opencv2/videoio.hpp>
#include <vector>

auto show_image() {
    auto path = "resources/test.png";
    auto img = cv::imread(path);

    cv::Mat imgGray, imgBlur, imgCanny;

    cv::cvtColor(img, imgGray, cv::COLOR_BGRA2GRAY);
    cv::GaussianBlur(img, imgBlur, cv::Size(7, 7), 0, 0);

    cv::Canny(imgBlur, imgCanny, 50, 150);

    cv::imshow("image", img);
    cv::imshow("gray image", imgGray);
    cv::imshow("blur image", imgBlur);
    cv::imshow("canny image", imgCanny);
    cv::waitKey();
}

auto basic_function() {
    auto path = "resources/test.png";
    auto img = cv::imread(path);

    cv::Mat imgResize, imgCrop;

    cv::resize(img, imgResize, cv::Size(640, 480));
    auto rect = cv::Rect(0, 0, 100, 100);
    imgCrop = img(rect);

    cv::imshow("image", img);
    cv::imshow("image resize", imgResize);
    cv::imshow("image crop", imgCrop);
    cv::waitKey();
}

auto shapes_and_text() {

    auto img = cv::Mat(512, 512, CV_8UC3, cv::Scalar(0xff, 0xff, 0xff));

    cv::circle(img, cv::Point(256, 256), 64, cv::Scalar(0xfa, 0, 0), 1);
    cv::rectangle(img, cv::Point(0, 0), cv::Point(100, 100),
                  cv::Scalar(0xfa, 0, 0), 2);
    cv::line(img, cv::Point(100, 100), cv::Point(200, 200),
             cv::Scalar(0xfa, 0, 0), 1);
    cv::putText(img, "Hello OpenCV", cv::Point(100, 200),
                cv::FONT_HERSHEY_DUPLEX, 1, cv::Scalar(0, 0, 0));
    cv::imshow("image", img);
    cv::waitKey();
}

auto warp() {
    auto path = "resources/cards.jpg";
    auto img = cv::imread(path);
    auto imgWarp = cv::Mat();
    constexpr float w = 250, h = 350;
    cv::Point2f src[4] = {{529., 142}, {771, 190}, {674, 457}, {405, 395}};
    cv::Point2f dst[4] = {{0, 0}, {w, 0}, {w, h}, {0, h}};
    auto matrix = cv::getPerspectiveTransform(src, dst);
    cv::warpPerspective(img, imgWarp, matrix, cv::Size(w, h));

    cv::imshow("img warp", imgWarp);
    cv::waitKey();
}

auto color_detect() {
    int lh = 0, ls = 110, lv = 153, uh = 19, us = 240, uv = 255;
    cv::namedWindow("Trackbars");
    cv::createTrackbar("lower h", "Trackbars", &lh, 179);
    cv::createTrackbar("lower s", "Trackbars", &ls, 255);
    cv::createTrackbar("lower v", "Trackbars", &lv, 255);
    cv::createTrackbar("upper h", "Trackbars", &uh, 179);
    cv::createTrackbar("upper s", "Trackbars", &us, 255);
    cv::createTrackbar("upper v", "Trackbars", &uv, 255);

    auto img = cv::imread("resources/lambo.png");
    auto imgHSV = cv::Mat(), mask = cv::Mat();
    cv::cvtColor(img, imgHSV, cv::COLOR_BGR2HSV);
    while (true) {
        cv::Scalar lower(lh, ls, lv);
        cv::Scalar upper(uh, us, uv);

        cv::inRange(imgHSV, lower, upper, mask);

        cv::imshow("img", img);
        cv::imshow("img HSV", imgHSV);
        cv::imshow("mask", mask);
        cv::waitKey(1);
    }
}

auto shape_detection() {
    auto img = cv::imread("resources/shapes.png");
    cv::Mat imgGray, imgBlur, imgCanny, imgDilate, imgThreshold;
    cv::cvtColor(img, imgGray, cv::COLOR_BGR2GRAY);
    cv::threshold(imgGray, imgThreshold, 254, 255, cv::THRESH_BINARY_INV);
    cv::GaussianBlur(imgGray, imgBlur, cv::Size(3, 3), 3, 0);
    cv::Canny(imgBlur, imgCanny, 25, 75);
    auto kernel = cv::getStructuringElement(cv::MORPH_RECT, cv::Size(3, 3));
    cv::dilate(imgCanny, imgDilate, kernel);
    std::vector<std::vector<cv::Point>> contours;
    std::vector<cv::Vec4i> hierachy;

    cv::findContours(imgThreshold, contours, hierachy, cv::RETR_LIST,
                     cv::CHAIN_APPROX_SIMPLE);
    std::vector<std::vector<cv::Point>> approxContours(contours.size());
    std::string type{};
    for (int i = 0; i < contours.size(); i++) {
        std::cout << "area" << std::endl;
        auto area = cv::contourArea(contours[i]);
        std::cout << area << std::endl;
        // std::cout << "length" << std::endl;
        auto length = cv::arcLength(contours[i], true);
        // std::cout << length << std::endl;

        // filiter
        if (area <= 400)
            continue;

        cv::approxPolyDP(contours[i], approxContours[i], 0.02 * length, true);
        auto rect = cv::boundingRect(contours[i]);
        cv::rectangle(img, rect, cv::Scalar(0, 255, 0), 2);
        cv::drawContours(img, contours, i, cv::Scalar(255, 0, 255), 5);
        cv::drawContours(img, approxContours, i, cv::Scalar(255, 0, 0), 2);

        switch (approxContours[i].size()) {
        case 3:
            type = "triangle";
            break;
        case 4:
            type = "rectangle";
            break;
        default:
            type = "circle";
            break;
        }
        cv::putText(img, type, {rect.x, rect.y - 5}, cv::FONT_HERSHEY_DUPLEX,
                    0.75, cv::Scalar(255, 0, 255));
    }
    cv::imshow("image", img);
    cv::imshow("image gray", imgGray);
    cv::imshow("image threshold", imgThreshold);
    cv::imshow("image blur", imgBlur);
    cv::imshow("image canny", imgCanny);
    cv::imshow("image dilate", imgDilate);
    cv::waitKey();
}

auto face_detection() {
    auto img = cv::Mat();
    cv::Mat imgGray;

    cv::CascadeClassifier faceCascade{};
    faceCascade.load("resources/haarcascade_frontalface_default.xml");
    std::vector<cv::Rect> objects{};

    auto video = cv::VideoCapture(0);
    video.set(cv::CAP_PROP_FOURCC, cv::VideoWriter::fourcc('M', 'J', 'P', 'G'));

    while (true) {
        video.read(img);
        cv::flip(img, img, 1);
        cv::cvtColor(img, imgGray, cv::COLOR_BGR2GRAY);
        faceCascade.detectMultiScale(imgGray, objects);
        for (auto &item : objects) {
            cv::rectangle(img, item, cv::Scalar(255, 0, 255), 2);
        }
        cv::imshow("image", img);
        cv::waitKey(20);
    }
}

auto show_video() {
    auto path = "resources/test_video.mp4";
    auto video = cv::VideoCapture(path);
    cv::Mat img;
    while (true) {
        video.read(img);
        cv::imshow("image", img);
        cv::waitKey(20);
    }
}

auto show_camare() {
    auto video = cv::VideoCapture(0, cv::CAP_V4L2);
    video.set(cv::CAP_PROP_FOURCC, cv::VideoWriter::fourcc('M', 'J', 'P', 'G'));

    video.set(cv::CAP_PROP_FRAME_WIDTH, 640);
    video.set(cv::CAP_PROP_FRAME_HEIGHT, 480);

    cv::Mat img;
    while (true) {
        video.read(img);
        cv::flip(img, img, 1);
        cv::imshow("image", img);
        cv::waitKey(1);
    }
}

auto project_detect_object() {
    auto video = cv::VideoCapture(0, cv::CAP_V4L2);
    video.set(cv::CAP_PROP_FOURCC, cv::VideoWriter::fourcc('M', 'J', 'P', 'G'));
    cv::Mat img, img_hsv, img_match;

    int hl = 158, sl = 64, vl = 98, hu = 179, su = 123, vu = 135;
    int min_area = 4000;
    cv::namedWindow("hsv_window");
    cv::createTrackbar("h min", "hsv_window", &hl, 179);
    cv::createTrackbar("h max", "hsv_window", &hu, 179);
    cv::createTrackbar("s min", "hsv_window", &sl, 255);
    cv::createTrackbar("s max", "hsv_window", &su, 255);
    cv::createTrackbar("v min", "hsv_window", &vl, 255);
    cv::createTrackbar("v max", "hsv_window", &vu, 255);

    cv::namedWindow("area_window");
    cv::createTrackbar("min area", "area_window", &min_area, 10240);

    std::vector<std::vector<cv::Point>> contours{};
    std::vector<std::vector<cv::Point>> approx_contours{};
    while (true) {
        video.read(img);
        cv::flip(img, img, 1);

        cv::cvtColor(img, img_hsv, cv::COLOR_BGR2HSV);
        cv::inRange(img_hsv, cv::Scalar(hl, sl, vl), cv::Scalar(hu, su, vu),
                    img_match);
        cv::findContours(img_match, contours, cv::RETR_LIST,
                         cv::CHAIN_APPROX_SIMPLE);
        approx_contours.resize(contours.size());
        for (int i = 0; i < contours.size(); i++) {
            if (cv::contourArea(contours[i]) <= static_cast<int>(min_area)) {
                continue;
            }
            cv::drawContours(img, contours, i, cv::Scalar(255, 0, 255));
            auto length = cv::arcLength(contours[i], true);
            cv::approxPolyDP(contours[i], approx_contours[i], 0.02 * length,
                             true);
            cv::drawContours(img, approx_contours, i, cv::Scalar(255, 0, 0));
            auto rect = cv::boundingRect(approx_contours[i]);
            cv::rectangle(img, rect, cv::Scalar(0, 255, 0));
        }
        cv::imshow("image", img);
        cv::imshow("image match", img_match);

        cv::waitKey(20);
    }
}

auto main() -> int {
    project_detect_object();
    return 0;
}
```