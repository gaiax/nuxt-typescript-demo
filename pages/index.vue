<template>
  <div>
    <h1>Search Google Books</h1>
    <input type="text" v-model="searchText" placeholder="Python入門" />
    <p>検索タイトル: {{ searchText }}</p>
    <ul>
      <li :key="volume.id" v-for="volume in volumes">
        <a :href="volume.volumeInfo.infoLink">{{ volume.volumeInfo.title }}</a>
      </li>
    </ul>
  </div>
</template>
<script lang="ts">
import axios from "axios";

type IndustryIdentifier = {
  type: string;
  identifier: string;
};

type ReadingModes = {
  text: boolean;
  image: boolean;
};

type PanelizationSummary = {
  containsEpubBubbles: boolean;
  containsImageBubbles: boolean;
};

type ImageLinks = {
  smallThumbnail: string;
  thumbnail: string;
};

type VolumeInfo = {
  title: string;
  authors: string[];
  publisher: string;
  publishedDate: string;
  description: string;
  industryIdentifiers: IndustryIdentifier[];
  readingModes: ReadingModes;
  pageCount: number;
  printType: string;
  categories: string[];
  maturityRating: string;
  allowAnonLogging: boolean;
  contentVersion: string;
  panelizationSummary: PanelizationSummary;
  imageLinks: ImageLinks;
  language: string;
  previewLink: string;
  infoLink: string;
  canonicalVolumeLink: string;
};

type ListPrice = {
  amount: number;
  currencyCode: string;
};

type RetailPrice = {
  amount: number;
  currencyCode: string;
};

type ListPrice2 = {
  amountInMicros: number;
  currencyCode: string;
};

type RetailPrice2 = {
  amountInMicros: number;
  currencyCode: string;
};

type Offer = {
  finskyOfferType: number;
  listPrice: ListPrice2;
  retailPrice: RetailPrice2;
};

type SaleInfo = {
  country: string;
  saleability: string;
  isEbook: boolean;
  listPrice: ListPrice;
  retailPrice: RetailPrice;
  buyLink: string;
  offers: Offer[];
};

type Epub = {
  isAvailable: boolean;
  acsTokenLink: string;
};

type Pdf = {
  isAvailable: boolean;
  acsTokenLink: string;
};

type AccessInfo = {
  country: string;
  viewability: string;
  embeddable: boolean;
  publicDomain: boolean;
  textToSpeechPermission: string;
  epub: Epub;
  pdf: Pdf;
  webReaderLink: string;
  accessViewStatus: string;
  quoteSharingAllowed: boolean;
};

type SearchInfo = {
  textSnippet: string;
};

type Volume = {
  kind: string;
  id: string;
  etag: string;
  selfLink: string;
  volumeInfo: VolumeInfo;
  saleInfo: SaleInfo;
  accessInfo: AccessInfo;
  searchInfo: SearchInfo;
};

export default {
  data() {
    return { searchText: "", volumes: [] };
  },
  methods: {
    async getVolumes(searchText: string) {
      this.volumes = await axios
        .get("https://www.googleapis.com/books/v1/volumes/?q=" + searchText)
        .then(res => {
          const items: Array<Volume> = res.data.items;
          return items;
        })
        .catch(e => {
          console.error({ statusCode: 404, message: "ページが見つかりません" });
        });
    }
  },
  watch: {
    searchText: function(newSearchText, oldSearchText) {
      this.getVolumes(newSearchText);
    }
  }
};
</script>

<style></style>
