# JoinUsBanner

![join-us-banner](../images/join-us-banner.png)

## Usage
```jsx
import { Grid } from '@material-ui/core';
import Button from '../../components/Button';
import JoinUsBanner from '../../components/partials/banner/JoinUsBanner';

class Page extends React.Component {
  render () {
    return (
      <JoinUsBanner
        image={{
          md: '/resources/corp/images/join-feature-desktop.jpg',
          sm: '/resources/corp/images/join-feature-mobile.png',
        }}
        title='完善的職涯發展'>
        <Grid container spacing={2}>
          <Grid item xs={12} sm={12} md={6}>
            <h4>遠傳電訊教育訓練</h4>
            <p>引進專業設備與經驗，以培養台灣電訊科技人才</p>
          </Grid>
          <Grid item xs={12} sm={12} md={6}>
            <h4>遠傳知識中心網站</h4>
            <p>提供豐富知識與趨勢資訊分享讓學習成長更多元</p>
          </Grid>
          <Grid item xs={12} sm={12} md={6}>
            <h4>核心職能訓練</h4>
            <p>核心職能訓練介紹</p>
          </Grid>
          <Grid item xs={12} sm={12} md={6}>
            <h4>新晉升主管訓練</h4>
            <p>新晉升主管訓練介紹</p>
          </Grid>
          <Grid item xs={12} sm={12} md={6}>
            <h4>新進人員訓練</h4>
            <p>核心職能訓練介紹</p>
          </Grid>
          <Grid item xs={12} sm={12} md={6}>
            <h4>專業技術訓練</h4>
            <p>專業技術訓練介紹</p>
          </Grid>
        </Grid>
        <Button btnStyle='secondary' className='mt-4'>
          樂業職場
        </Button>
      </JoinUsBanner>
    )
  }
}
```

## Source
```jsx
import React from 'react';
import { Grid } from '@material-ui/core';
import PropTypes from 'prop-types';

class JoinUsBanner extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      isMobile: window.innerWidth < 960,
    };
  }

  componentDidMount() {
    window.addEventListener('resize', (e) => {
      this.setState({
        isMobile: window.innerWidth < 768,
      });
    });
  }

  render() {
    return (
      <section
        className='fui-joinus-banner'
        style={{
          backgroundImage: `url(${this.state.isMobile ? this.props.image.sm : this.props.image.md})`,
        }}>
        <div className='fui-container'>
          <div className='fui-banner-content'>
            <h2>{this.props.title}</h2>
            {this.props.children}
          </div>
        </div>
      </section>
    );
  }
}

JoinUsBanner.propTypes = {
  image: PropTypes.shape({
    md: PropTypes.string,
    sm: PropTypes.string,
  }),
  title: PropTypes.string,
  children: PropTypes.any,
};

export default JoinUsBanner;
```

## Properties
| 名稱 |  屬性 | 必填 | 選項 | 說明 |
| :--- | :--- | :--- | :--- | :--- |
| image | Object | true |  | **md:** 大網圖<br/>**sm:** 小網圖 |
| title | String | true |  | 標題 |
| children | Any | true |  | 放需要的 HTML 內容 |