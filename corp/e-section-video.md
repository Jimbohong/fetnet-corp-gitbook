# ESectionVideo

![e-section-video](../images/e-section-video.png)

### Usage
```jsx
import ESectionVideo from '../../components/partials/ESectionVideo';

const Video = [
  {
    image: 'https://placeimg.com/1600/900/any',
    title: '遠傳以 Less is more 落實節能行動',
    link: 'https://youtu.be/XDN2wbm6zi8',
  },
  ...
]

class Page extends React.Component {
  constructor (props) {
    super(props)
    this.state = {
      canLoadMoreVideo: true,
      currentPage: 0,
      videos: Video
    }
  }

  videoLoadMore = () => {
    console.log(this.state.videos);
    this.setState({
      videos: [...this.state.videos, ...Mock.loadMoreVideo],
      canLoadMoreVideo: false,
      currentPage: this.state.currentPage + 1,
    });
    // console.log(this.state.videos);
  };

  render () {
    return (
      <section className='pt-0'>
        <div className='fui-container'>
          <ESectionVideo
            title='CSR 相關影片'
            cards={this.state.videos}
            loadMore={this.videoLoadMore}
            totalPage={2}
            currentPage={this.state.currentPage}
          />
        </div>
      </section>
    )
  }
}
```

### Source
```jsx
import React from 'react';
import VideoModal from '../VideoModal'
import LoadMore from '../LoadMore'
import PropTypes from 'prop-types';

const Card = props => {
  return (
    <div className={`fui-card ${props.className ? props.className : ''}`}>
      <div className='fui-card-action' onClick={e => props.onClick(props)}>
        {props.image ? (
          <div className='fui-card-image'>
            <img src={process.env.PUBLIC_URL + props.image} srcSet={process.env.PUBLIC_URL + (props.retinaImage || props.image)} alt={props.meta} />
          </div>
        ) : (
            ''
          )}
        <div className='fui-card-caption'>
          <div className='fui-card-content'>
            {!props.meta ? '' : <div className='fui-card-meta'>{props.meta}</div>}
            <h4 className='fui-card-title'>{props.title}</h4>
            {!props.description ? (
              ''
            ) : (
                <p className='fui-card-description' dangerouslySetInnerHTML={{ __html: props.description }}></p>
              )}
          </div>
          <div className='fui-card-extra'>
            <div
              className={`fui-button is-arrow mb-0 ${
                props.className && props.className.indexOf('promotion-article') > -1 ? 'is-reverse' : ''
                }`}>
              {props.action ? props.action.text : props.isEn ? 'More' : '看更多'}
            </div>
          </div>
        </div>
      </div>
    </div>
  );
};

class ESectionVideo extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      isEn: false,
      currentPage: 0,
      currentVideo: '',
      modalOpen: false,
      alterVideo: '',
      currentArticleLoadMore: false,
      cards: this.props.cards,
    };

    this.loadMore = this.loadMore.bind(this);
  }

  componentDidMount() {
    this.setState({
      currentArticleLoadMore: this.props.totalPage > 0,
    });
  }

  componentDidUpdate() {
    // 偵測 cards 改變，加入新的 cards
    if (this.props.currentPage !== this.state.currentPage) {
      let oldCard = Object.assign(this.state.cards);
      this.setState({ cards: [] });
      // debugger
      this.setState({
        currentPage: this.props.currentPage,
        currentArticleLoadMore: this.props.currentPage !== this.props.totalPage,
        cards: oldCard.concat(this.props.cards),
      });
    }

    if ((typeof window !== 'undefined' && window.$isEn) !== this.state.isEn) {
      this.setState({
        isEn: typeof window !== 'undefined' && window.$isEn
      })
    }
  }

  loadMore = () => {
    // call API 取得更多影片，透過 props 傳回新的 card
    if (this.props.loadMore) this.props.loadMore(this.state.currentPage);
  };

  openVideoModal = data => {
    console.log(data);

    this.setState({
      modalOpen: true,
      alterVideo: data.alterVideo,
      currentVideo: data.link,
    });
  };

  closeModal = () => {
    this.setState({
      modalOpen: false,
      alterVideo: '',
      currentVideo: '',
    });
  };
  render() {
    return (
      <section className={`fui-content video-container ${this.props.className ? this.props.className : ''}`}>
        <div className='video-head'>
          <h2 className='m-0'>{this.props.title}</h2>
          <button className='fui-button' onClick={() => console.log('go to somewhere')}>
            <span className='text'>{this.state.isEn ? 'More' : '看更多'}</span>
            <i className='icon-chevron-right' />
          </button>
        </div>
        <div className={`fui-cards ${this.props.column}-card is-video`}>
          {this.state.cards.length
            ? this.state.cards.map((card, i) => (
              <Card key={`video-card-${i}`} className='is-video' {...card} isEn={this.state.isEn} onClick={this.openVideoModal} />
            ))
            : ''}
        </div>

        {this.props.totalPage > 0 ? (
          <LoadMore
            moreLabel={this.state.isEn ? 'More' : '展開看更多'}
            noMoreLabel={this.state.isEn ? 'No More Vidoe' : '沒有更多影片'}
            click={this.loadMore}
            load={this.state.currentArticleLoadMore}
          />
        ) : (
            ''
          )}
        <VideoModal
          open={this.state.modalOpen}
          alterVideo={this.state.alterVideo}
          videoUrl={this.state.currentVideo}
          onClose={this.closeModal}
        />
      </section>
    );
  }
}

ESectionVideo.defaultProps = {
  column: 'three',
};
ESectionVideo.propTypes = {
  title: PropTypes.string.isRequired,
  cards: PropTypes.arrayOf(Card).isRequired,
  className: PropTypes.string,
  totalPage: PropTypes.number,
  currentPage: PropTypes.number,
  column: PropTypes.string, // two or three
  loadMore: PropTypes.func, // get more cards
};

export default ESectionVideo;
```

## Properties
| 名稱 | 屬性 | 必填 | 選項 | 說明 |
| :--- | :--- | :--- | :--- | :--- |
| title | String | true |  | 標題 |
| cards | String | true |  | **image:** 影片封面<br>**title:** 影片標題<br>**link:** 影片連結 |
| className | String |  |  | 客製化樣式 |
| totalPage | Number |  |  | 資料總頁數 |
| currentPage | Number |  |  | 目前頁數 |
| column | Number |  |  | 一列顯示牌卡數，可設定 2~4 |
| loadMore | Function |  |  | 呼叫 API 取得更多影片 |
