
![BannerPromotionBasic](../images/banner-promotion-basic.png)

## Usage
```jsx
import BannerPromotionBasic from '../../components/partials/banner/BannerPromotionBasic';

const Slide = [
  {
    theme: 'dark',
    image: {
      md: '/resources/corp/images/cbu_joinus-1920x500.jpg',
      sm: '/resources/corp/images/cbu-joinus-750x900.jpg',
    },
    title: '和遠傳一起翻轉未來行動生活',
    description: '為了走在5G時代的最前面，我們由衷希望，你和公司一同成長。',
    actions: [
      { text: '填履歷表', link: '/', btnStyle: 'primary' },
      { text: '查詢職缺', link: '/', btnStyle: 'secondary', reverse: true },
    ],
  },
];

class Page extends React.Component {
  render() {
    return (
      <BannerPromotionBasic
        slides={Slide}
      />
    )
  }
}
```

## Source
```jsx
import React from 'react';
import Slider from 'react-slick';
import Button from '../../Button';
import Modal from 'react-modal';

import PropTypes from 'prop-types';

class BannerPromotionBasic extends React.Component {
  constructor(props) {
    super(props);

    this.slideSetting = {
      dots: true,
      infinite: true,
      arrows: false,
      slidesToShow: 1,
      autoplay: this.props.autoplay ? this.props.autoplay : false,
      autoplaySpeed: this.props.autoplaySpeed ? this.props.autoplaySpeed : 5000,
      draggable: true,
      adaptiveHeight: true,
      afterChange: current => this.setState({ activeSlide: current })
    };
    this.state = {
      modalOpen: false,
      activeSlide: 0,
    }
  }

  setModalContent = (content) => {
    this.props.openModal({
      type: 'exclusive-promotion',
      ...content
    })
  }
  getThemeColor = () => {
    let current = this.state.activeSlide;
    return this.props.slides[current].theme === 'dark' ? 'dark-theme' : '';
  }
  render() {
    return (
      <div className={`fui-banner is-exclusive is-promotion ${this.getThemeColor()} ${this.props.className ? this.props.className : ''}`}>
        <Slider {...this.slideSetting}>
          {
            this.props.slides.map((slide, i) => (
              <div className={`estore-banner-item ${slide.theme === 'dark' ? 'dark-theme' : ''}`} key={`estore-banner-${i}`}>
                <div className='bg' style={this.props.bgColor ? { backgroundColor: this.props.bgColor } : null}>
                  <img src={slide.image.md} alt={slide.title} className='d-none d-sm-block' />
                  <img src={slide.image.sm} alt={slide.title} className='d-block d-sm-none' />
                </div>
                <div className='caption'>
                  <div className='fui-container'>
                    <div className="caption-wrapper">
                      {!!slide.tag ? <div className='tag'>
                        <div className='tag-bg'></div>
                        {slide.tag}
                      </div> : null}
                      <h1 dangerouslySetInnerHTML={{
                        __html: slide.title
                      }} />
                      <div className="description" dangerouslySetInnerHTML={{
                        __html: slide.description
                      }} />
                      <div className='action p-0'>
                        {
                          slide.actions.map((act, j) => (
                            act.type === 'link' ? (
                              <Button link={act.link} target={act.target} btnStyle={act.btnStyle} key={`estore-banner-${i}-btn-${j}`} size='large'>
                                {act.text}
                              </Button>
                            ) : (
                                <Button onClick={e => this.setModalContent(act.modalContent)} btnStyle={act.btnStyle} key={`estore-banner-${i}-btn-${j}`} size='large'>
                                  {act.text}
                                </Button>
                              )
                          ))
                        }
                      </div>
                    </div>
                  </div>
                </div>

                <div className='fui-arrow d-block d-sm-none'>
                  <i className="icon-chevron-down"></i>
                </div>
              </div>
            ))
          }
        </Slider>
      </div>
    )
  }
}

BannerPromotionBasic.propTypes = {
  slides: PropTypes.arrayOf(
    PropTypes.shape({
      image: PropTypes.shape({
        md: PropTypes.string,
        sm: PropTypes.string
      }),
      theme: PropTypes.string,
      tag: PropTypes.string,
      title: PropTypes.string,
      description: PropTypes.string,
      actions: PropTypes.arrayOf(
        PropTypes.shape({
          btnStyle: PropTypes.string,
          modalContent: PropTypes.string,
          text: PropTypes.string,
          link: PropTypes.string,
          target: PropTypes.string,
        })
      )
    }).isRequired
  ),
  className: PropTypes.string,
  bgColor: PropTypes.string,
  openModal: PropTypes.func,
}

export default BannerPromotionBasic;
```


## Properties
| 名稱 | 內容 |  屬性 | 必填 | 選項 | 說明 |
| :--- | :--- | :--- | :--- | :--- | :--- |
| slides |  | Array | true |  | 輪播 |
| - | image | Object | true |  | **md:** 大網圖<br/>**sm:** 小網圖 |
| - | theme | String |  |  | 'dark-theme' 會顯示白字，預設為黑字 |
| - | tag | String | true |  | 標題上方說明小字 |
| - | title | String | true |  | 標題 |
| - | description | String | true |  | 說明 |
| - | actions | Object | true |  | **btnStyle:** 按鈕樣式，可選擇 `primary` 或 `secondary`<br>**text:** 連結文字<br/>**link:** 連結<br/>**target:** _self or _blank<br>**modalContent:** {title: modal 標題, content: modal 內容} |
| className |  | String | true |  | 客製化樣式 |
| bgColor |  | String | true |  | 背景顏色，直接輸入色碼 |
| openModal |  | Function |  |  | 呼叫 modal 並顯示內容 |