# SimpleBanner

![simple-banner](../images/simple-banner.png)

### Usage
```jsx
import SimpleBanner from '../../components/partials/banner/SimpleBanner';

class Page extends React.Component {
  render() {
    return (
      <SimpleBanner
        image={{
          md: '/resources/corp/images/about-aboutfet-1920-x-360@2x.jpg',
          sm: '/resources/corp/images/about-aboutfet-750-x-400@2x.jpg',
        }}
        title='子公司/關係企業'
      />
    )
  }
}
```

### Source 
```jsx
import React from 'react';
import Link from '../../Link';

import PropTypes from 'prop-types';

class SimpleBanner extends React.Component {
  constructor(props) {
    super(props);

    this.state = {
      isMobile: window.innerWidth < 768,
    };
  }

  componentDidMount = () => {
    if (window === 'undefined') return;

    window.addEventListener('resize', (e) => {
      this.setState({
        isMobile: window.innerWidth < 768,
      });
    });
  };

  render() {
    return (
      <section
        className='fui-banner is-find-product lazyload'
        id={this.props.id || ''}
        data-bg={process.env.PUBLIC_URL + (this.state.isMobile ? this.props.image.sm : this.props.image.md)}
        style={{
          backgroundImage: `url(${
            process.env.PUBLIC_URL + (this.state.isMobile ? this.props.image.sm : this.props.image.md)
          })`,
        }}>
        <div className='fui-container' dangerouslySetInnerHTML={{ __html: this.props.title }}></div>
      </section>
    );
  }
}

SimpleBanner.propTypes = {
  title: PropTypes.string,
  image: PropTypes.shape({
    md: PropTypes.string,
    sm: PropTypes.string,
  }),
};

export default SimpleBanner;
```

## Properties
| 名稱 | 屬性 | 必填 | 選項 | 說明 |
| :--- | :--- | :--- | :--- | :--- |
| title | String | true |  | 標題，必須輸入完整 HTML，如：\<h1>標題\</h1> |
| image | Object | true |  | **md:** 大網圖片 <br/>**sm:** 小網圖片 |