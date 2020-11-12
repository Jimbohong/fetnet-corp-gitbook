# FilterCard

![filter-card](../images/filter-card.png)

### Usage
```jsx
import FilterCard from '../../components/partials/card/FilterCard';

class Page extends React.Compnent {
  render () {
    return (
      <FilterCard
        title='CSR相關內容'
        cards={[
          {
            link: 'https://www.fetnet.net/corporate/20191026.html',
            image: 'https://www.fetnet.net/corporate/images/tc/beneficent/20191026_1.jpg',
            title: '遠傳認養南北海灘 號召百名同仁淨灘',
            public_at: '2019/10/26',
            meta: '環境教育',
          },
          ...
        ]}
      />
    )
  }
}
```

### Source
```jsx
import React from 'react';
import { Grid } from '@material-ui/core';
import Card from '../../card/Card';
import LoadMore from '../../LoadMore';
import Dropdown from '../../Dropdown';

import PropTypes from 'prop-types';

class FilterCard extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      category: '',
      year: '',
      month: '',
      filterCards: this.props.cards,
      totalPage: 0,
      currentPage: 0,
      cards: [],
      currentArticleLoadMore: false,
      perPage: this.props.perPage || 9,
    };
  }

  componentDidMount = () => {
    let total = Math.floor(this.props.cards.length / this.state.perPage);
    this.setState({
      totalPage: total,
      cards: this.props.cards.splice(this.state.currentPage, (this.state.currentPage + 1) * this.state.perPage),
      currentArticleLoadMore: total !== this.state.currentPage,
    });
  };

  componentDidUpdate(prevProps, prevState) {
    if (prevProps.cards !== this.props.cards) {
      let total = Math.floor(this.props.cards.length / this.state.perPage);
      this.setState({
        filterCards: this.props.cards,
        currentPage: 0,
        cards: [
          ...this.state.cards,
          ...this.props.cards.splice(this.state.currentPage, (this.state.currentPage + 1) * this.state.perPage),
        ],
        totalPage: total,
        currentArticleLoadMore: this.props.currentPage !== this.props.totalPage,
      });
    }
  }

  loadMore = () => {
    const { filterCards } = this.state;
    this.setState({
      currentPage: this.state.currentPage + 1,
      cards: [
        ...this.state.cards,
        ...filterCards.splice(this.state.currentPage + 1, (this.state.currentPage + 2) * this.state.perPage),
      ],
      currentArticleLoadMore: this.state.totalPage !== this.state.currentPage + 1,
    });
  };

  changeFilter = (name, item, idx, e) => {
    this.setState({
      [name]: item.value,
      currentPage: 0,
    });

    this.forceUpdate();

    let cards =
      this.state.category === '' && this.state.year === '' && this.state.month === ''
        ? this.props.cards
        : this.props.cards.reduce((accr, card, i, array) => {
            if (
              card.meta.indexOf(this.state.category) > -1 ||
              card.public_at.indexOf(this.state.year) > -1 ||
              card.public_at.indexOf(this.state.year + '/' + this.state.month) > -1
            ) {
              accr.push(card);
            }
            return accr;
          }, []);

    let total = Math.floor(cards.length / this.state.perPage);
    this.setState({
      filterCards: cards,
      totalPage: total,
      cards: [
        ...this.state.cards,
        cards.splice(this.state.currentPage, (this.state.currentPage + 1) * this.state.perPage),
      ],
      currentArticleLoadMore: total !== this.state.currentPage,
    });
  };

  render() {
    return (
      <section>
        <div className='fui-container'>
          <h2>{this.props.title}</h2>
          <Grid container spacing={2} className='mb-2'>
            <Grid item xs={12} sm={4} md={2}>
              <Dropdown
                list={[
                  { text: '類別', value: '' },
                  { text: '環境教育', value: '環境教育' },
                  { text: '數位包容', value: '數位包容' },
                  { text: '社會共好', value: '社會共好' },
                ]}
                className='is-button'
                arrow={true}
                label={this.state.category === '' ? '類別' : this.state.category}
                onChange={(item, idx, e) => this.changeFilter('category', item, idx, e)}
              />
            </Grid>
            <Grid item xs={12} sm={4} md={2}>
              <Dropdown
                list={[
                  { text: '年份', value: '' },
                  { text: '2019', value: '2019' },
                  { text: '2018', value: '2018' },
                  { text: '2017', value: '2017' },
                ]}
                className='is-button'
                arrow={true}
                label={this.state.year === '' ? '年份' : this.state.year}
                onChange={(item, idx, e) => this.changeFilter('year', item, idx, e)}
              />
            </Grid>
            <Grid item xs={12} sm={4} md={2}>
              <Dropdown
                list={[
                  { text: '月份', value: '' },
                  { text: '環境教育', value: '環境教育' },
                  { text: '數位包容', value: '數位包容' },
                  { text: '社會共好', value: '社會共好' },
                ]}
                className='is-button'
                arrow={true}
                label={this.state.month === '' ? '月份' : this.state.month}
                onChange={(item, idx, e) => this.changeFilter('month', item, idx, e)}
              />
            </Grid>
          </Grid>
          <div className='fui-cards three-card no-scrollbar is-filter-cards'>
            {this.state.cards.length
              ? this.state.cards.map((card, i) => <Card key={`filter-card-${i}`} {...card} />)
              : null}
          </div>
          {this.state.totalPage > 0 ? (
            <LoadMore
              moreLabel={this.state.isEn ? 'More' : '展開看更多'}
              noMoreLabel={this.state.isEn ? 'No More Content' : '沒有更多內容'}
              click={this.loadMore}
              load={this.state.currentArticleLoadMore}
            />
          ) : null}
        </div>
      </section>
    );
  }
}

FilterCard.propTypes = {
  title: PropTypes.string,
  perPage: PropTypes.number,
  cards: PropTypes.arrayOf(
    PropTypes.shape({
      link: PropTypes.string,
      image: PropTypes.string,
      public_at: PropTypes.string,
      meta: PropTypes.string,
      title: PropTypes.string,
      description: PropTypes.string,
    })
  ),
};

export default FilterCard;
```


## Properties
| 名稱 | 屬性 | 必填 | 選項 | 說明 |
| :--- | :--- | :--- | :--- | :--- |
| title | String | true |  | 標題 |
| perPage | Number |  |  | 一次要顯示的牌卡數，預設為 9 |
| cards | Array | true | | link: 牌卡連結<br/>image: 圖片<br/>public_at: 發布日期<br/>meta: 分類或說明<br/>title: 標題<br/>description: 簡述 |