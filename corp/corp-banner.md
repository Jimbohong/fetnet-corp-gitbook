# CorpBanner

![corp-banner](../images/cbu-banner.png)

## Usage
```jsx
import CorpBanner from '../components/partials/banner/CorpBanner';

const CorpBanner = {
  image: {
    md: '/resources/corp/images/about-home-1920x500.jpg',
    sm: '/resources/corp/images/about-home-750x900.jpg',
  },
  title: '靠得更近，想得更遠',
  description: '遠傳致力拉近人與人的距離',
  action: {
    text: '了解遠傳',
    link: '/',
    target: '_blank',
  },
};

class Page extends React.Component {
  render () { 
    return (
      <CorpBanner {...CorpBanner} />
    )
  }
    
}
```

## Source
```jsx
import React from 'react';
import Button from '../../Button';

import PropTypes from 'prop-types';

function detectWidth() {
  return window.innerWidth < 768;
}

class CorpBanner extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      isMobile: detectWidth(),
    };
  }

  componentDidMount = () => {
    window.addEventListener('resize', () => {
      this.setState({
        isMobile: detectWidth(),
      });
    });
  };

  render() {
    return (
      <div
        className='fui-banner is-corp'
        style={{
          backgroundImage: `url(${this.state.isMobile ? this.props.image.sm : this.props.image.md})`,
        }}>
        <div className='caption'>
          <div className='fui-container'>
            <h1 className='with-quote'>{this.props.title}</h1>
            <h4>{this.props.description}</h4>
            <Button link={this.props.action.link} target={this.props.action.target} btnStyle='secondary' reverse={true}>
              {this.props.action.text}
            </Button>
          </div>
        </div>
      </div>
    );
  }
}

CorpBanner.propTypes = {
  image: {
    md: PropTypes.string,
    sm: PropTypes.string,
  },
  title: PropTypes.string,
  description: PropTypes.string,
  action: PropTypes.shape({
    link: PropTypes.string,
    text: PropTypes.string,
    target: PropTypes.string,
  }),
};

export default CorpBanner;
```


## Properties
| 名稱 | 屬性 | 必填 | 選項 | 說明 |
| :--- | :--- | :--- | :--- | :--- |
| image | Object | true |  | md: 大網圖片, sm: 小網圖片 |
| title | String | true |  | 標題 |
| description | String | true |  | 描述 |
| action | Object | true |  | **link:** 連結, <br/>**text:** 按鈕文字, <br/>**target:** _self \| _blank (換頁或另開視窗) |