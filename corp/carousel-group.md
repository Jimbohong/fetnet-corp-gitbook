# CarouselGroup

![carousel-group](../images/carousel-group.png)

## Usage
```jsx
import CarouselGroup from '../components/partials/carousel/CarouselGroup';

const CorpFeature = {
  title: '秉持品牌精神，實踐企業社會責任',
  description:
    '遠傳電信於 2017 年再次盤點產業趨勢、風險與機會，正式確立永續發展策略，期能極大化企業對經濟、環境及社會的貢獻，達到「生活有遠傳電信、溝通無距離、人生更豐富」的企業願景，成為消費者數位生活好夥伴。',
  action: {
    text: '下載 2019 整合性報告書',
    link: '/',
    target: '_blank',
  },
  list: [
    {
      image: '/resources/corp/images/corporate-social-responsibility-3-1@2x.jpg',
      name: '公司治理',
      content:
        '遠傳電信始終以創新之經營理念，落實完善公司治理、監督架構及誠信經營原則，構成促進企業長期獲利、創造價值的基石',
      link: '',
    },
   ...
  ],
};

class Page extends React.Component {
  render () {
    return (
      <CarouselGroup {...CorpFeature} />
    )
  }
}
```

## Source
```jsx
import React from 'react';
import { Grid } from '@material-ui/core';

import Slider from 'react-slick';
import Link from '../../Link';
import Button from '../../Button';
import ArrowLeftWhite from '../../animateArrow/ArrowLeftWhite';

import PropTypes from 'prop-types';

class CarouselGroup extends React.Component {
  constructor(props) {
    super(props);

    this.prevSlider = React.createRef();
    this.nextSlider = React.createRef();
    this.state = {
      settings: {
        infinite: true,
        speed: 1000,
        autoplaySpeed: 7000,
        slidesToShow: 1,
        slidesToScroll: 1,
        pauseOnHover: true,
      },
      prevData: this.setPrevSlide(),
      nextData: this.setNextSlide(),
    };
  }

  setPrevSlide = () => {
    if(this.props.list.length === 0){
      return [];
    }
    let prevData = [...this.props.list];
    let last = prevData.pop();
    prevData = [last].concat(prevData);
    return prevData;
  };

  setNextSlide = () => {
    if(this.props.list.length === 0){
      return [];
    }
    let nextData = [...this.props.list];
    let first = nextData.shift();
    nextData.push(first);
    return nextData;
  };

  updateSlider = (oldIndex, newIndex) => {
    if (window.innerWidth < 768) return;

    this.prevSlider.current.slickGoTo(newIndex);
    this.nextSlider.current.slickGoTo(newIndex);
  };

  render() {
    const { settings } = this.state;

    return (
      <section className='fui-carousel-group'>
        <ArrowLeftWhite />
        <div className='fui-container'>
          <h2>{this.props.title}</h2>
          <div className='subtitle' dangerouslySetInnerHTML={{ __html: this.props.description }}></div>
          <Grid container>
            <Grid item md={3} className='carousen-container'>
              <Slider className='shop-carousel is-prev' ref={this.prevSlider} {...settings} arrows={false}>
                {this.state.prevData.map((item, idx) => (
                  <div key={`carousel-${idx}`} align='center' className='shop-carousel-item'>
                    <div className='bg' style={{ backgroundImage: `url(${item.image})` }}></div>
                    <p>{item.name} </p>
                  </div>
                ))}
              </Slider>
            </Grid>
            <Grid item md={6} className='carousen-container'>
              <Slider
                className='shop-carousel is-main'
                dots={true}
                arrows={true}
                beforeChange={this.updateSlider}
                {...settings}>
                {this.props.list.map((item, idx) => (
                  <div key={`carousel-${idx}`} className='shop-carousel-item'>
                    <div className='bg' style={{ backgroundImage: `url(${item.image})` }}></div>
                    <div className='content'>
                      <h2>{item.name}</h2>
                      <div className='subtitle'>{item.content}</div>
                    </div>
                  </div>
                ))}
              </Slider>
            </Grid>
            <Grid item md={3} className='carousen-container'>
              <Slider className='shop-carousel is-next' ref={this.nextSlider} {...settings} arrows={false}>
                {this.state.nextData.map((item, idx) => (
                  <div key={`carousel-${idx}`} align='center' className='shop-carousel-item'>
                    <div className='bg' style={{ backgroundImage: `url(${item.image})` }}></div>
                    <p>{item.name} </p>
                  </div>
                ))}
              </Slider>
            </Grid>
          </Grid>
          <div className='pt-3'>
            <Button btnStyle='secondary' link={this.props.action.link} target={this.props.action.target}>
              {this.props.action.text}
            </Button>
          </div>
        </div>
      </section>
    );
  }
}

CarouselGroup.propTypes = {
  title: PropTypes.string,
  description: PropTypes.string,
  action: PropTypes.shape({
    lingk: PropTypes.string,
    target: PropTypes.string,
    text: PropTypes.string,
  }),
  list: PropTypes.arrayOf(
    PropTypes.shape({
      image: PropTypes.string,
      name: PropTypes.string,
      content: PropTypes.string,
      link: PropTypes.string,
    })
  ),
};

export default CarouselGroup;
```

## Properties
| 名稱 | 屬性 | 必填 | 選項 | 說明 |
| :--- | :--- | :--- | :--- | :--- |
| title | String |  |  | 標題 |
| description | String |  |  | 描述 |
| action | Object | true |  | **link:** 連結, <br/>**text:** 按鈕文字, <br/>**target:** _self \| _blank (換頁或另開視窗) |
| list | Array | true |  | Banner 內容：{<br/>**image:** 背景圖, <br/>**name:** Banner 標題, <br/>**content:** Banner 內容, <br/>**link:** Banner 連結<br/>} |
