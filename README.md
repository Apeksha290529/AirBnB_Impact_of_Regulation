# AirBnB_Impact_of_Regulation
{
 "cells": [
  {
   "cell_type": "code",
   "execution_count": 1,
   "id": "dcc23ffd",
   "metadata": {
    "_cell_guid": "b1076dfc-b9ad-4769-8c92-a6c4dae69d19",
    "_uuid": "8f2839f25d086af736a60e9eeb907d3b93b6e0e5",
    "execution": {
     "iopub.execute_input": "2025-02-10T03:28:07.946959Z",
     "iopub.status.busy": "2025-02-10T03:28:07.946730Z",
     "iopub.status.idle": "2025-02-10T03:28:08.656231Z",
     "shell.execute_reply": "2025-02-10T03:28:08.655231Z"
    },
    "papermill": {
     "duration": 0.721934,
     "end_time": "2025-02-10T03:28:08.657736",
     "exception": false,
     "start_time": "2025-02-10T03:28:07.935802",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "/kaggle/input/airbnb-listings-reviews/Airbnb Data/Listings.csv\n",
      "/kaggle/input/airbnb-listings-reviews/Airbnb Data/Reviews.csv\n",
      "/kaggle/input/airbnb-listings-reviews/Airbnb Data/Reviews_data_dictionary.csv\n",
      "/kaggle/input/airbnb-listings-reviews/Airbnb Data/Listings_data_dictionary.csv\n"
     ]
    }
   ],
   "source": [
    "# This Python 3 environment comes with many helpful analytics libraries installed\n",
    "# It is defined by the kaggle/python Docker image: https://github.com/kaggle/docker-python\n",
    "# For example, here's several helpful packages to load\n",
    "\n",
    "import numpy as np # linear algebra\n",
    "import pandas as pd # data processing, CSV file I/O (e.g. pd.read_csv)\n",
    "\n",
    "# Input data files are available in the read-only \"../input/\" directory\n",
    "# For example, running this (by clicking run or pressing Shift+Enter) will list all files under the input directory\n",
    "\n",
    "import os\n",
    "for dirname, _, filenames in os.walk('/kaggle/input'):\n",
    "    for filename in filenames:\n",
    "        print(os.path.join(dirname, filename))\n",
    "\n",
    "# You can write up to 20GB to the current directory (/kaggle/working/) that gets preserved as output when you create a version using \"Save & Run All\" \n",
    "# You can also write temporary files to /kaggle/temp/, but they won't be saved outside of the current session"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "a2689358",
   "metadata": {
    "papermill": {
     "duration": 0.008762,
     "end_time": "2025-02-10T03:28:08.675952",
     "exception": false,
     "start_time": "2025-02-10T03:28:08.667190",
     "status": "completed"
    },
    "tags": []
   },
   "source": [
    "<div style=\"background-color: #f88379; padding: 25px; border-radius: 10px;font-size:20px;font-weight:10px\">\n",
    "  <h1 style=\"color: #333;\"><strong>AirBnB Listings and reviews</strong></h1>\n",
    "</div>"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "7fd3a7e1",
   "metadata": {
    "papermill": {
     "duration": 0.008523,
     "end_time": "2025-02-10T03:28:08.693223",
     "exception": false,
     "start_time": "2025-02-10T03:28:08.684700",
     "status": "completed"
    },
    "tags": []
   },
   "source": [
    "<img src=\"https://jayvas.com/wp-content/uploads/2020/11/airbnb-real-estate-company.jpg\" width=1200>"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "ec12132d",
   "metadata": {
    "papermill": {
     "duration": 0.008909,
     "end_time": "2025-02-10T03:28:08.711180",
     "exception": false,
     "start_time": "2025-02-10T03:28:08.702271",
     "status": "completed"
    },
    "tags": []
   },
   "source": [
    "<div style=\"background-color: #0000; padding: 20px; border-radius: 10px;font-size:25px;font-weight:10px\">\n",
    "  <h1 style=\"color: #333;\"><strong>Exploratory Data Analysis</strong></h1>\n",
    "</div>"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 2,
   "id": "dc77895d",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2025-02-10T03:28:08.729571Z",
     "iopub.status.busy": "2025-02-10T03:28:08.729175Z",
     "iopub.status.idle": "2025-02-10T03:28:08.741380Z",
     "shell.execute_reply": "2025-02-10T03:28:08.740590Z"
    },
    "papermill": {
     "duration": 0.022948,
     "end_time": "2025-02-10T03:28:08.742759",
     "exception": false,
     "start_time": "2025-02-10T03:28:08.719811",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [],
   "source": [
    "reviews_dict = pd.read_csv(r\"/kaggle/input/airbnb-listings-reviews/Airbnb Data/Reviews_data_dictionary.csv\")"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 3,
   "id": "3ee5774b",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2025-02-10T03:28:08.761139Z",
     "iopub.status.busy": "2025-02-10T03:28:08.760938Z",
     "iopub.status.idle": "2025-02-10T03:28:08.777621Z",
     "shell.execute_reply": "2025-02-10T03:28:08.776917Z"
    },
    "papermill": {
     "duration": 0.027105,
     "end_time": "2025-02-10T03:28:08.778852",
     "exception": false,
     "start_time": "2025-02-10T03:28:08.751747",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Field</th>\n",
       "      <th>Description</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>listing_id</td>\n",
       "      <td>Listing ID</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>review_id</td>\n",
       "      <td>Review ID</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>date</td>\n",
       "      <td>Review date</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>reviewer_id</td>\n",
       "      <td>Reviewer ID</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "         Field  Description\n",
       "0   listing_id   Listing ID\n",
       "1    review_id    Review ID\n",
       "2         date  Review date\n",
       "3  reviewer_id  Reviewer ID"
      ]
     },
     "execution_count": 3,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "reviews_dict"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 4,
   "id": "9549b799",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2025-02-10T03:28:08.797649Z",
     "iopub.status.busy": "2025-02-10T03:28:08.797425Z",
     "iopub.status.idle": "2025-02-10T03:28:12.581770Z",
     "shell.execute_reply": "2025-02-10T03:28:12.581089Z"
    },
    "papermill": {
     "duration": 3.795419,
     "end_time": "2025-02-10T03:28:12.583425",
     "exception": false,
     "start_time": "2025-02-10T03:28:08.788006",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [],
   "source": [
    "reviews = pd.read_csv(r\"/kaggle/input/airbnb-listings-reviews/Airbnb Data/Reviews.csv\")"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 5,
   "id": "dea8a0a3",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2025-02-10T03:28:12.602718Z",
     "iopub.status.busy": "2025-02-10T03:28:12.602436Z",
     "iopub.status.idle": "2025-02-10T03:28:12.610078Z",
     "shell.execute_reply": "2025-02-10T03:28:12.609279Z"
    },
    "papermill": {
     "duration": 0.018166,
     "end_time": "2025-02-10T03:28:12.611281",
     "exception": false,
     "start_time": "2025-02-10T03:28:12.593115",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>listing_id</th>\n",
       "      <th>review_id</th>\n",
       "      <th>date</th>\n",
       "      <th>reviewer_id</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>11798</td>\n",
       "      <td>330265172</td>\n",
       "      <td>2018-09-30</td>\n",
       "      <td>11863072</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>15383</td>\n",
       "      <td>330103585</td>\n",
       "      <td>2018-09-30</td>\n",
       "      <td>39147453</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>16455</td>\n",
       "      <td>329985788</td>\n",
       "      <td>2018-09-30</td>\n",
       "      <td>1125378</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>17919</td>\n",
       "      <td>330016899</td>\n",
       "      <td>2018-09-30</td>\n",
       "      <td>172717984</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>26827</td>\n",
       "      <td>329995638</td>\n",
       "      <td>2018-09-30</td>\n",
       "      <td>17542859</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "   listing_id  review_id        date  reviewer_id\n",
       "0       11798  330265172  2018-09-30     11863072\n",
       "1       15383  330103585  2018-09-30     39147453\n",
       "2       16455  329985788  2018-09-30      1125378\n",
       "3       17919  330016899  2018-09-30    172717984\n",
       "4       26827  329995638  2018-09-30     17542859"
      ]
     },
     "execution_count": 5,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "reviews.head()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 6,
   "id": "d882af3a",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2025-02-10T03:28:12.630915Z",
     "iopub.status.busy": "2025-02-10T03:28:12.630707Z",
     "iopub.status.idle": "2025-02-10T03:28:12.636796Z",
     "shell.execute_reply": "2025-02-10T03:28:12.636220Z"
    },
    "papermill": {
     "duration": 0.017569,
     "end_time": "2025-02-10T03:28:12.637958",
     "exception": false,
     "start_time": "2025-02-10T03:28:12.620389",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [],
   "source": [
    "listings_dict = pd.read_csv(r\"/kaggle/input/airbnb-listings-reviews/Airbnb Data/Listings_data_dictionary.csv\")"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 7,
   "id": "74e65264",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2025-02-10T03:28:12.656848Z",
     "iopub.status.busy": "2025-02-10T03:28:12.656652Z",
     "iopub.status.idle": "2025-02-10T03:28:12.659532Z",
     "shell.execute_reply": "2025-02-10T03:28:12.658922Z"
    },
    "papermill": {
     "duration": 0.013592,
     "end_time": "2025-02-10T03:28:12.660646",
     "exception": false,
     "start_time": "2025-02-10T03:28:12.647054",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [],
   "source": [
    "import warnings\n",
    "warnings.filterwarnings('ignore')"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 8,
   "id": "5176dc3a",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2025-02-10T03:28:12.679230Z",
     "iopub.status.busy": "2025-02-10T03:28:12.679035Z",
     "iopub.status.idle": "2025-02-10T03:28:12.686629Z",
     "shell.execute_reply": "2025-02-10T03:28:12.685934Z"
    },
    "papermill": {
     "duration": 0.018308,
     "end_time": "2025-02-10T03:28:12.687870",
     "exception": false,
     "start_time": "2025-02-10T03:28:12.669562",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Field</th>\n",
       "      <th>Description</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>listing_id</td>\n",
       "      <td>Listing ID</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>name</td>\n",
       "      <td>Listing Name</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>host_id</td>\n",
       "      <td>Host ID</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>host_since</td>\n",
       "      <td>Date the Host joined Airbnb</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>host_location</td>\n",
       "      <td>Location where the Host is based</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>5</th>\n",
       "      <td>host_response_time</td>\n",
       "      <td>Estimate of how long the Host takes to respond</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>6</th>\n",
       "      <td>host_response_rate</td>\n",
       "      <td>Percentage of times the Host responds</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>7</th>\n",
       "      <td>host_acceptance_rate</td>\n",
       "      <td>Percentage of times the Host accepts a booking...</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>8</th>\n",
       "      <td>host_is_superhost</td>\n",
       "      <td>Binary field to determine if the Host is a Sup...</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>9</th>\n",
       "      <td>host_total_listings_count</td>\n",
       "      <td>Total listings the Host has in Airbnb</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>10</th>\n",
       "      <td>host_has_profile_pic</td>\n",
       "      <td>Binary field to determine if the Host has a pr...</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>11</th>\n",
       "      <td>host_identity_verified</td>\n",
       "      <td>Binary field to determine if the Host has a ve...</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>12</th>\n",
       "      <td>neighbourhood</td>\n",
       "      <td>Neighborhood the Listing is in</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>13</th>\n",
       "      <td>district</td>\n",
       "      <td>District the Listing is in</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>14</th>\n",
       "      <td>city</td>\n",
       "      <td>City the Listing is in</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>15</th>\n",
       "      <td>latitude</td>\n",
       "      <td>Listing's latitude</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>16</th>\n",
       "      <td>longitude</td>\n",
       "      <td>Listing's longitude</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>17</th>\n",
       "      <td>property_type</td>\n",
       "      <td>Type of property for the Listing</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>18</th>\n",
       "      <td>room_type</td>\n",
       "      <td>Type of room type in Airbnb for the Listing</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>19</th>\n",
       "      <td>accommodates</td>\n",
       "      <td>Guests the Listing accomodates</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>20</th>\n",
       "      <td>bedrooms</td>\n",
       "      <td>Bedrooms in the Listing</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>21</th>\n",
       "      <td>amenities</td>\n",
       "      <td>Amenities the Listing includes</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>22</th>\n",
       "      <td>price</td>\n",
       "      <td>Listing price (in each country's currency)</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>23</th>\n",
       "      <td>minimum_nights</td>\n",
       "      <td>Minimum nights per booking</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>24</th>\n",
       "      <td>maximum_nights</td>\n",
       "      <td>Maximum nights per booking</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>25</th>\n",
       "      <td>review_scores_rating</td>\n",
       "      <td>Listing's overall rating (out of 100)</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>26</th>\n",
       "      <td>review_scores_accuracy</td>\n",
       "      <td>Listing's accuracy score based on what's promo...</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>27</th>\n",
       "      <td>review_scores_cleanliness</td>\n",
       "      <td>Listing's cleanliness score (out of 10)</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>28</th>\n",
       "      <td>review_scores_checkin</td>\n",
       "      <td>Listing's check-in experience score (out of 10)</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>29</th>\n",
       "      <td>review_scores_communication</td>\n",
       "      <td>Listing's communication with the Host score (o...</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>30</th>\n",
       "      <td>review_scores_location</td>\n",
       "      <td>Listing's location score within the city (out ...</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>31</th>\n",
       "      <td>review_scores_value</td>\n",
       "      <td>Listing's value score relative to its price (o...</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>32</th>\n",
       "      <td>instant_bookable</td>\n",
       "      <td>Binary field to determine if the Listing can b...</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "                          Field  \\\n",
       "0                    listing_id   \n",
       "1                          name   \n",
       "2                       host_id   \n",
       "3                    host_since   \n",
       "4                 host_location   \n",
       "5            host_response_time   \n",
       "6            host_response_rate   \n",
       "7          host_acceptance_rate   \n",
       "8             host_is_superhost   \n",
       "9     host_total_listings_count   \n",
       "10         host_has_profile_pic   \n",
       "11       host_identity_verified   \n",
       "12                neighbourhood   \n",
       "13                     district   \n",
       "14                         city   \n",
       "15                     latitude   \n",
       "16                    longitude   \n",
       "17                property_type   \n",
       "18                    room_type   \n",
       "19                 accommodates   \n",
       "20                     bedrooms   \n",
       "21                    amenities   \n",
       "22                        price   \n",
       "23               minimum_nights   \n",
       "24               maximum_nights   \n",
       "25         review_scores_rating   \n",
       "26       review_scores_accuracy   \n",
       "27    review_scores_cleanliness   \n",
       "28        review_scores_checkin   \n",
       "29  review_scores_communication   \n",
       "30       review_scores_location   \n",
       "31          review_scores_value   \n",
       "32             instant_bookable   \n",
       "\n",
       "                                          Description  \n",
       "0                                          Listing ID  \n",
       "1                                        Listing Name  \n",
       "2                                             Host ID  \n",
       "3                         Date the Host joined Airbnb  \n",
       "4                    Location where the Host is based  \n",
       "5      Estimate of how long the Host takes to respond  \n",
       "6               Percentage of times the Host responds  \n",
       "7   Percentage of times the Host accepts a booking...  \n",
       "8   Binary field to determine if the Host is a Sup...  \n",
       "9               Total listings the Host has in Airbnb  \n",
       "10  Binary field to determine if the Host has a pr...  \n",
       "11  Binary field to determine if the Host has a ve...  \n",
       "12                     Neighborhood the Listing is in  \n",
       "13                         District the Listing is in  \n",
       "14                             City the Listing is in  \n",
       "15                                 Listing's latitude  \n",
       "16                                Listing's longitude  \n",
       "17                   Type of property for the Listing  \n",
       "18        Type of room type in Airbnb for the Listing  \n",
       "19                     Guests the Listing accomodates  \n",
       "20                            Bedrooms in the Listing  \n",
       "21                     Amenities the Listing includes  \n",
       "22         Listing price (in each country's currency)  \n",
       "23                         Minimum nights per booking  \n",
       "24                         Maximum nights per booking  \n",
       "25              Listing's overall rating (out of 100)  \n",
       "26  Listing's accuracy score based on what's promo...  \n",
       "27            Listing's cleanliness score (out of 10)  \n",
       "28    Listing's check-in experience score (out of 10)  \n",
       "29  Listing's communication with the Host score (o...  \n",
       "30  Listing's location score within the city (out ...  \n",
       "31  Listing's value score relative to its price (o...  \n",
       "32  Binary field to determine if the Listing can b...  "
      ]
     },
     "execution_count": 8,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "listings_dict"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "3d6bd9e1",
   "metadata": {
    "papermill": {
     "duration": 0.009322,
     "end_time": "2025-02-10T03:28:12.706577",
     "exception": false,
     "start_time": "2025-02-10T03:28:12.697255",
     "status": "completed"
    },
    "tags": []
   },
   "source": [
    "<div style=\"background-color: #f88379; padding: 25px; border-radius: 10px;\">\n",
    "  <h1 style=\"color: #333;\"><strong>Loading and Checking the dataset</strong></h1>\n",
    "</div>"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 9,
   "id": "f73488ac",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2025-02-10T03:28:12.725584Z",
     "iopub.status.busy": "2025-02-10T03:28:12.725322Z",
     "iopub.status.idle": "2025-02-10T03:28:16.833699Z",
     "shell.execute_reply": "2025-02-10T03:28:16.832707Z"
    },
    "papermill": {
     "duration": 4.119738,
     "end_time": "2025-02-10T03:28:16.835445",
     "exception": false,
     "start_time": "2025-02-10T03:28:12.715707",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [],
   "source": [
    "listings = pd.read_csv(r\"/kaggle/input/airbnb-listings-reviews/Airbnb Data/Listings.csv\",encoding = \"ISO-8859-1\",low_memory=False)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 10,
   "id": "d3760412",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2025-02-10T03:28:16.855743Z",
     "iopub.status.busy": "2025-02-10T03:28:16.855476Z",
     "iopub.status.idle": "2025-02-10T03:28:16.879379Z",
     "shell.execute_reply": "2025-02-10T03:28:16.878594Z"
    },
    "papermill": {
     "duration": 0.035307,
     "end_time": "2025-02-10T03:28:16.880717",
     "exception": false,
     "start_time": "2025-02-10T03:28:16.845410",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>listing_id</th>\n",
       "      <th>name</th>\n",
       "      <th>host_id</th>\n",
       "      <th>host_since</th>\n",
       "      <th>host_location</th>\n",
       "      <th>host_response_time</th>\n",
       "      <th>host_response_rate</th>\n",
       "      <th>host_acceptance_rate</th>\n",
       "      <th>host_is_superhost</th>\n",
       "      <th>host_total_listings_count</th>\n",
       "      <th>...</th>\n",
       "      <th>minimum_nights</th>\n",
       "      <th>maximum_nights</th>\n",
       "      <th>review_scores_rating</th>\n",
       "      <th>review_scores_accuracy</th>\n",
       "      <th>review_scores_cleanliness</th>\n",
       "      <th>review_scores_checkin</th>\n",
       "      <th>review_scores_communication</th>\n",
       "      <th>review_scores_location</th>\n",
       "      <th>review_scores_value</th>\n",
       "      <th>instant_bookable</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>281420</td>\n",
       "      <td>Beautiful Flat in le Village Montmartre, Paris</td>\n",
       "      <td>1466919</td>\n",
       "      <td>2011-12-03</td>\n",
       "      <td>Paris, Ile-de-France, France</td>\n",
       "      <td>NaN</td>\n",
       "      <td>NaN</td>\n",
       "      <td>NaN</td>\n",
       "      <td>f</td>\n",
       "      <td>1.0</td>\n",
       "      <td>...</td>\n",
       "      <td>2</td>\n",
       "      <td>1125</td>\n",
       "      <td>100.0</td>\n",
       "      <td>10.0</td>\n",
       "      <td>10.0</td>\n",
       "      <td>10.0</td>\n",
       "      <td>10.0</td>\n",
       "      <td>10.0</td>\n",
       "      <td>10.0</td>\n",
       "      <td>f</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>3705183</td>\n",
       "      <td>39 mÃÂ² Paris (Sacre CÃ",
       "âur)</td>\n",
       "      <td>10328771</td>\n",
       "      <td>2013-11-29</td>\n",
       "      <td>Paris, Ile-de-France, France</td>\n",
       "      <td>NaN</td>\n",
       "      <td>NaN</td>\n",
       "      <td>NaN</td>\n",
       "      <td>f</td>\n",
       "      <td>1.0</td>\n",
       "      <td>...</td>\n",
       "      <td>2</td>\n",
       "      <td>1125</td>\n",
       "      <td>100.0</td>\n",
       "      <td>10.0</td>\n",
       "      <td>10.0</td>\n",
       "      <td>10.0</td>\n",
       "      <td>10.0</td>\n",
       "      <td>10.0</td>\n",
       "      <td>10.0</td>\n",
       "      <td>f</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>4082273</td>\n",
       "      <td>Lovely apartment with Terrace, 60m2</td>\n",
       "      <td>19252768</td>\n",
       "      <td>2014-07-31</td>\n",
       "      <td>Paris, Ile-de-France, France</td>\n",
       "      <td>NaN</td>\n",
       "      <td>NaN</td>\n",
       "      <td>NaN</td>\n",
       "      <td>f</td>\n",
       "      <td>1.0</td>\n",
       "      <td>...</td>\n",
       "      <td>2</td>\n",
       "      <td>1125</td>\n",
       "      <td>100.0</td>\n",
       "      <td>10.0</td>\n",
       "      <td>10.0</td>\n",
       "      <td>10.0</td>\n",
       "      <td>10.0</td>\n",
       "      <td>10.0</td>\n",
       "      <td>10.0</td>\n",
       "      <td>f</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>4797344</td>\n",
       "      <td>Cosy studio (close to Eiffel tower)</td>\n",
       "      <td>10668311</td>\n",
       "      <td>2013-12-17</td>\n",
       "      <td>Paris, Ile-de-France, France</td>\n",
       "      <td>NaN</td>\n",
       "      <td>NaN</td>\n",
       "      <td>NaN</td>\n",
       "      <td>f</td>\n",
       "      <td>1.0</td>\n",
       "      <td>...</td>\n",
       "      <td>2</td>\n",
       "      <td>1125</td>\n",
       "      <td>100.0</td>\n",
       "      <td>10.0</td>\n",
       "      <td>10.0</td>\n",
       "      <td>10.0</td>\n",
       "      <td>10.0</td>\n",
       "      <td>10.0</td>\n",
       "      <td>10.0</td>\n",
       "      <td>f</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>4823489</td>\n",
       "      <td>Close to Eiffel Tower - Beautiful flat : 2 rooms</td>\n",
       "      <td>24837558</td>\n",
       "      <td>2014-12-14</td>\n",
       "      <td>Paris, Ile-de-France, France</td>\n",
       "      <td>NaN</td>\n",
       "      <td>NaN</td>\n",
       "      <td>NaN</td>\n",
       "      <td>f</td>\n",
       "      <td>1.0</td>\n",
       "      <td>...</td>\n",
       "      <td>2</td>\n",
       "      <td>1125</td>\n",
       "      <td>100.0</td>\n",
       "      <td>10.0</td>\n",
       "      <td>10.0</td>\n",
       "      <td>10.0</td>\n",
       "      <td>10.0</td>\n",
       "      <td>10.0</td>\n",
       "      <td>10.0</td>\n",
       "      <td>f</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "<p>5 rows × 33 columns</p>\n",
       "</div>"
      ],
      "text/plain": [
       "   listing_id                                              name   host_id  \\\n",
       "0      281420    Beautiful Flat in le Village Montmartre, Paris   1466919   \n",
       "1     3705183                   39 mÃÂ² Paris (Sacre CÃ\n",
       "âur)  10328771   \n",
       "2     4082273               Lovely apartment with Terrace, 60m2  19252768   \n",
       "3     4797344               Cosy studio (close to Eiffel tower)  10668311   \n",
       "4     4823489  Close to Eiffel Tower - Beautiful flat : 2 rooms  24837558   \n",
       "\n",
       "   host_since                 host_location host_response_time  \\\n",
       "0  2011-12-03  Paris, Ile-de-France, France                NaN   \n",
       "1  2013-11-29  Paris, Ile-de-France, France                NaN   \n",
       "2  2014-07-31  Paris, Ile-de-France, France                NaN   \n",
       "3  2013-12-17  Paris, Ile-de-France, France                NaN   \n",
       "4  2014-12-14  Paris, Ile-de-France, France                NaN   \n",
       "\n",
       "   host_response_rate  host_acceptance_rate host_is_superhost  \\\n",
       "0                 NaN                   NaN                 f   \n",
       "1                 NaN                   NaN                 f   \n",
       "2                 NaN                   NaN                 f   \n",
       "3                 NaN                   NaN                 f   \n",
       "4                 NaN                   NaN                 f   \n",
       "\n",
       "   host_total_listings_count  ... minimum_nights maximum_nights  \\\n",
       "0                        1.0  ...              2           1125   \n",
       "1                        1.0  ...              2           1125   \n",
       "2                        1.0  ...              2           1125   \n",
       "3                        1.0  ...              2           1125   \n",
       "4                        1.0  ...              2           1125   \n",
       "\n",
       "  review_scores_rating review_scores_accuracy review_scores_cleanliness  \\\n",
       "0                100.0                   10.0                      10.0   \n",
       "1                100.0                   10.0                      10.0   \n",
       "2                100.0                   10.0                      10.0   \n",
       "3                100.0                   10.0                      10.0   \n",
       "4                100.0                   10.0                      10.0   \n",
       "\n",
       "   review_scores_checkin  review_scores_communication review_scores_location  \\\n",
       "0                   10.0                         10.0                   10.0   \n",
       "1                   10.0                         10.0                   10.0   \n",
       "2                   10.0                         10.0                   10.0   \n",
       "3                   10.0                         10.0                   10.0   \n",
       "4                   10.0                         10.0                   10.0   \n",
       "\n",
       "  review_scores_value  instant_bookable  \n",
       "0                10.0                 f  \n",
       "1                10.0                 f  \n",
       "2                10.0                 f  \n",
       "3                10.0                 f  \n",
       "4                10.0                 f  \n",
       "\n",
       "[5 rows x 33 columns]"
      ]
     },
     "execution_count": 10,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "listings.head()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 11,
   "id": "6f54b35e",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2025-02-10T03:28:16.901062Z",
     "iopub.status.busy": "2025-02-10T03:28:16.900827Z",
     "iopub.status.idle": "2025-02-10T03:28:17.100368Z",
     "shell.execute_reply": "2025-02-10T03:28:17.099221Z"
    },
    "papermill": {
     "duration": 0.211052,
     "end_time": "2025-02-10T03:28:17.101717",
     "exception": false,
     "start_time": "2025-02-10T03:28:16.890665",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "<class 'pandas.core.frame.DataFrame'>\n",
      "RangeIndex: 279712 entries, 0 to 279711\n",
      "Data columns (total 33 columns):\n",
      " #   Column                       Non-Null Count   Dtype  \n",
      "---  ------                       --------------   -----  \n",
      " 0   listing_id                   279712 non-null  int64  \n",
      " 1   name                         279537 non-null  object \n",
      " 2   host_id                      279712 non-null  int64  \n",
      " 3   host_since                   279547 non-null  object \n",
      " 4   host_location                278872 non-null  object \n",
      " 5   host_response_time           150930 non-null  object \n",
      " 6   host_response_rate           150930 non-null  float64\n",
      " 7   host_acceptance_rate         166625 non-null  float64\n",
      " 8   host_is_superhost            279547 non-null  object \n",
      " 9   host_total_listings_count    279547 non-null  float64\n",
      " 10  host_has_profile_pic         279547 non-null  object \n",
      " 11  host_identity_verified       279547 non-null  object \n",
      " 12  neighbourhood                279712 non-null  object \n",
      " 13  district                     37012 non-null   object \n",
      " 14  city                         279712 non-null  object \n",
      " 15  latitude                     279712 non-null  float64\n",
      " 16  longitude                    279712 non-null  float64\n",
      " 17  property_type                279712 non-null  object \n",
      " 18  room_type                    279712 non-null  object \n",
      " 19  accommodates                 279712 non-null  int64  \n",
      " 20  bedrooms                     250277 non-null  float64\n",
      " 21  amenities                    279712 non-null  object \n",
      " 22  price                        279712 non-null  int64  \n",
      " 23  minimum_nights               279712 non-null  int64  \n",
      " 24  maximum_nights               279712 non-null  int64  \n",
      " 25  review_scores_rating         188307 non-null  float64\n",
      " 26  review_scores_accuracy       187999 non-null  float64\n",
      " 27  review_scores_cleanliness    188047 non-null  float64\n",
      " 28  review_scores_checkin        187941 non-null  float64\n",
      " 29  review_scores_communication  188025 non-null  float64\n",
      " 30  review_scores_location       187937 non-null  float64\n",
      " 31  review_scores_value          187927 non-null  float64\n",
      " 32  instant_bookable             279712 non-null  object \n",
      "dtypes: float64(13), int64(6), object(14)\n",
      "memory usage: 70.4+ MB\n"
     ]
    }
   ],
   "source": [
    "listings.info()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 12,
   "id": "f3238a04",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2025-02-10T03:28:17.122315Z",
     "iopub.status.busy": "2025-02-10T03:28:17.122097Z",
     "iopub.status.idle": "2025-02-10T03:28:17.170654Z",
     "shell.execute_reply": "2025-02-10T03:28:17.170060Z"
    },
    "papermill": {
     "duration": 0.060105,
     "end_time": "2025-02-10T03:28:17.171894",
     "exception": false,
     "start_time": "2025-02-10T03:28:17.111789",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [],
   "source": [
    "listings[\"host_since\"] = pd.to_datetime(listings[\"host_since\"]) # converting string into date"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "c1e2f9bb",
   "metadata": {
    "papermill": {
     "duration": 0.009332,
     "end_time": "2025-02-10T03:28:17.190827",
     "exception": false,
     "start_time": "2025-02-10T03:28:17.181495",
     "status": "completed"
    },
    "tags": []
   },
   "source": [
    "<div style=\"background-color: #f88379; padding: 25px; border-radius: 10px;\">\n",
    "  <h1 style=\"color: #333;\"><strong>Information About the dataset</strong></h1>\n",
    "</div>"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 13,
   "id": "2418f75e",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2025-02-10T03:28:17.210285Z",
     "iopub.status.busy": "2025-02-10T03:28:17.210074Z",
     "iopub.status.idle": "2025-02-10T03:28:17.390886Z",
     "shell.execute_reply": "2025-02-10T03:28:17.389825Z"
    },
    "papermill": {
     "duration": 0.192185,
     "end_time": "2025-02-10T03:28:17.392404",
     "exception": false,
     "start_time": "2025-02-10T03:28:17.200219",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "<class 'pandas.core.frame.DataFrame'>\n",
      "RangeIndex: 279712 entries, 0 to 279711\n",
      "Data columns (total 33 columns):\n",
      " #   Column                       Non-Null Count   Dtype         \n",
      "---  ------                       --------------   -----         \n",
      " 0   listing_id                   279712 non-null  int64         \n",
      " 1   name                         279537 non-null  object        \n",
      " 2   host_id                      279712 non-null  int64         \n",
      " 3   host_since                   279547 non-null  datetime64[ns]\n",
      " 4   host_location                278872 non-null  object        \n",
      " 5   host_response_time           150930 non-null  object        \n",
      " 6   host_response_rate           150930 non-null  float64       \n",
      " 7   host_acceptance_rate         166625 non-null  float64       \n",
      " 8   host_is_superhost            279547 non-null  object        \n",
      " 9   host_total_listings_count    279547 non-null  float64       \n",
      " 10  host_has_profile_pic         279547 non-null  object        \n",
      " 11  host_identity_verified       279547 non-null  object        \n",
      " 12  neighbourhood                279712 non-null  object        \n",
      " 13  district                     37012 non-null   object        \n",
      " 14  city                         279712 non-null  object        \n",
      " 15  latitude                     279712 non-null  float64       \n",
      " 16  longitude                    279712 non-null  float64       \n",
      " 17  property_type                279712 non-null  object        \n",
      " 18  room_type                    279712 non-null  object        \n",
      " 19  accommodates                 279712 non-null  int64         \n",
      " 20  bedrooms                     250277 non-null  float64       \n",
      " 21  amenities                    279712 non-null  object        \n",
      " 22  price                        279712 non-null  int64         \n",
      " 23  minimum_nights               279712 non-null  int64         \n",
      " 24  maximum_nights               279712 non-null  int64         \n",
      " 25  review_scores_rating         188307 non-null  float64       \n",
      " 26  review_scores_accuracy       187999 non-null  float64       \n",
      " 27  review_scores_cleanliness    188047 non-null  float64       \n",
      " 28  review_scores_checkin        187941 non-null  float64       \n",
      " 29  review_scores_communication  188025 non-null  float64       \n",
      " 30  review_scores_location       187937 non-null  float64       \n",
      " 31  review_scores_value          187927 non-null  float64       \n",
      " 32  instant_bookable             279712 non-null  object        \n",
      "dtypes: datetime64[ns](1), float64(13), int64(6), object(13)\n",
      "memory usage: 70.4+ MB\n"
     ]
    }
   ],
   "source": [
    "listings.info()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 14,
   "id": "42437394",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2025-02-10T03:28:17.412950Z",
     "iopub.status.busy": "2025-02-10T03:28:17.412719Z",
     "iopub.status.idle": "2025-02-10T03:28:17.418128Z",
     "shell.execute_reply": "2025-02-10T03:28:17.417490Z"
    },
    "papermill": {
     "duration": 0.016929,
     "end_time": "2025-02-10T03:28:17.419328",
     "exception": false,
     "start_time": "2025-02-10T03:28:17.402399",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "0         Paris\n",
       "1         Paris\n",
       "2         Paris\n",
       "3         Paris\n",
       "4         Paris\n",
       "          ...  \n",
       "279707    Paris\n",
       "279708    Paris\n",
       "279709    Paris\n",
       "279710    Paris\n",
       "279711    Paris\n",
       "Name: city, Length: 279712, dtype: object"
      ]
     },
     "execution_count": 14,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "listings[\"city\"]"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 15,
   "id": "11c2dbe3",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2025-02-10T03:28:17.439580Z",
     "iopub.status.busy": "2025-02-10T03:28:17.439371Z",
     "iopub.status.idle": "2025-02-10T03:28:17.456592Z",
     "shell.execute_reply": "2025-02-10T03:28:17.455930Z"
    },
    "papermill": {
     "duration": 0.028477,
     "end_time": "2025-02-10T03:28:17.457693",
     "exception": false,
     "start_time": "2025-02-10T03:28:17.429216",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "city\n",
       "Paris             64690\n",
       "New York          37012\n",
       "Sydney            33630\n",
       "Rome              27647\n",
       "Rio de Janeiro    26615\n",
       "Istanbul          24519\n",
       "Mexico City       20065\n",
       "Bangkok           19361\n",
       "Cape Town         19086\n",
       "Hong Kong          7087\n",
       "Name: count, dtype: int64"
      ]
     },
     "execution_count": 15,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "listings[\"city\"].value_counts()"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "73e0cca2",
   "metadata": {
    "papermill": {
     "duration": 0.009686,
     "end_time": "2025-02-10T03:28:17.477281",
     "exception": false,
     "start_time": "2025-02-10T03:28:17.467595",
     "status": "completed"
    },
    "tags": []
   },
   "source": [
    "<div style=\"background-color: #f88379; padding: 25px; border-radius: 10px;\">\n",
    "  <h1 style=\"color: #333;\"><strong>Columns and Rows Filtering</strong></h1>\n",
    "</div>"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 16,
   "id": "35ad03c0",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2025-02-10T03:28:17.497505Z",
     "iopub.status.busy": "2025-02-10T03:28:17.497264Z",
     "iopub.status.idle": "2025-02-10T03:28:17.527244Z",
     "shell.execute_reply": "2025-02-10T03:28:17.526629Z"
    },
    "papermill": {
     "duration": 0.041583,
     "end_time": "2025-02-10T03:28:17.528580",
     "exception": false,
     "start_time": "2025-02-10T03:28:17.486997",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [],
   "source": [
    "paris_listings = listings.query(\"city=='Paris'\")"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "91f9308f",
   "metadata": {
    "papermill": {
     "duration": 0.010196,
     "end_time": "2025-02-10T03:28:17.548940",
     "exception": false,
     "start_time": "2025-02-10T03:28:17.538744",
     "status": "completed"
    },
    "tags": []
   },
   "source": [
    "<div style=\"background-color: #f88379; padding: 25px; border-radius: 10px;\">\n",
    "  <h1 style=\"color: #333;\"><strong>Paris City Data Analysis</strong></h1>\n",
    "</div>"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 17,
   "id": "52b058e6",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2025-02-10T03:28:17.569600Z",
     "iopub.status.busy": "2025-02-10T03:28:17.569311Z",
     "iopub.status.idle": "2025-02-10T03:28:17.619960Z",
     "shell.execute_reply": "2025-02-10T03:28:17.619077Z"
    },
    "papermill": {
     "duration": 0.062935,
     "end_time": "2025-02-10T03:28:17.621727",
     "exception": false,
     "start_time": "2025-02-10T03:28:17.558792",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "<class 'pandas.core.frame.DataFrame'>\n",
      "Index: 64690 entries, 0 to 279711\n",
      "Data columns (total 33 columns):\n",
      " #   Column                       Non-Null Count  Dtype         \n",
      "---  ------                       --------------  -----         \n",
      " 0   listing_id                   64690 non-null  int64         \n",
      " 1   name                         64627 non-null  object        \n",
      " 2   host_id                      64690 non-null  int64         \n",
      " 3   host_since                   64657 non-null  datetime64[ns]\n",
      " 4   host_location                64522 non-null  object        \n",
      " 5   host_response_time           23346 non-null  object        \n",
      " 6   host_response_rate           23346 non-null  float64       \n",
      " 7   host_acceptance_rate         31919 non-null  float64       \n",
      " 8   host_is_superhost            64657 non-null  object        \n",
      " 9   host_total_listings_count    64657 non-null  float64       \n",
      " 10  host_has_profile_pic         64657 non-null  object        \n",
      " 11  host_identity_verified       64657 non-null  object        \n",
      " 12  neighbourhood                64690 non-null  object        \n",
      " 13  district                     0 non-null      object        \n",
      " 14  city                         64690 non-null  object        \n",
      " 15  latitude                     64690 non-null  float64       \n",
      " 16  longitude                    64690 non-null  float64       \n",
      " 17  property_type                64690 non-null  object        \n",
      " 18  room_type                    64690 non-null  object        \n",
      " 19  accommodates                 64690 non-null  int64         \n",
      " 20  bedrooms                     51286 non-null  float64       \n",
      " 21  amenities                    64690 non-null  object        \n",
      " 22  price                        64690 non-null  int64         \n",
      " 23  minimum_nights               64690 non-null  int64         \n",
      " 24  maximum_nights               64690 non-null  int64         \n",
      " 25  review_scores_rating         48036 non-null  float64       \n",
      " 26  review_scores_accuracy       47989 non-null  float64       \n",
      " 27  review_scores_cleanliness    47998 non-null  float64       \n",
      " 28  review_scores_checkin        47972 non-null  float64       \n",
      " 29  review_scores_communication  47991 non-null  float64       \n",
      " 30  review_scores_location       47971 non-null  float64       \n",
      " 31  review_scores_value          47972 non-null  float64       \n",
      " 32  instant_bookable             64690 non-null  object        \n",
      "dtypes: datetime64[ns](1), float64(13), int64(6), object(13)\n",
      "memory usage: 16.8+ MB\n"
     ]
    }
   ],
   "source": [
    "paris_listings.info()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 18,
   "id": "ca248730",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2025-02-10T03:28:17.645171Z",
     "iopub.status.busy": "2025-02-10T03:28:17.644917Z",
     "iopub.status.idle": "2025-02-10T03:28:17.679460Z",
     "shell.execute_reply": "2025-02-10T03:28:17.678553Z"
    },
    "papermill": {
     "duration": 0.048041,
     "end_time": "2025-02-10T03:28:17.680862",
     "exception": false,
     "start_time": "2025-02-10T03:28:17.632821",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [],
   "source": [
    "paris_listings = listings.query(\"city=='Paris'\").loc[:,[\"host_since\",\"neighbourhood\",\"city\",\"accommodates\",\"price\"]]"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 19,
   "id": "5eb6e3c2",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2025-02-10T03:28:17.701821Z",
     "iopub.status.busy": "2025-02-10T03:28:17.701593Z",
     "iopub.status.idle": "2025-02-10T03:28:17.709506Z",
     "shell.execute_reply": "2025-02-10T03:28:17.708722Z"
    },
    "papermill": {
     "duration": 0.019751,
     "end_time": "2025-02-10T03:28:17.710759",
     "exception": false,
     "start_time": "2025-02-10T03:28:17.691008",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>host_since</th>\n",
       "      <th>neighbourhood</th>\n",
       "      <th>city</th>\n",
       "      <th>accommodates</th>\n",
       "      <th>price</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>2011-12-03</td>\n",
       "      <td>Buttes-Montmartre</td>\n",
       "      <td>Paris</td>\n",
       "      <td>2</td>\n",
       "      <td>53</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>2013-11-29</td>\n",
       "      <td>Buttes-Montmartre</td>\n",
       "      <td>Paris</td>\n",
       "      <td>2</td>\n",
       "      <td>120</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>2014-07-31</td>\n",
       "      <td>Elysee</td>\n",
       "      <td>Paris</td>\n",
       "      <td>2</td>\n",
       "      <td>89</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>2013-12-17</td>\n",
       "      <td>Vaugirard</td>\n",
       "      <td>Paris</td>\n",
       "      <td>2</td>\n",
       "      <td>58</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>2014-12-14</td>\n",
       "      <td>Passy</td>\n",
       "      <td>Paris</td>\n",
       "      <td>2</td>\n",
       "      <td>60</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "  host_since      neighbourhood   city  accommodates  price\n",
       "0 2011-12-03  Buttes-Montmartre  Paris             2     53\n",
       "1 2013-11-29  Buttes-Montmartre  Paris             2    120\n",
       "2 2014-07-31             Elysee  Paris             2     89\n",
       "3 2013-12-17          Vaugirard  Paris             2     58\n",
       "4 2014-12-14              Passy  Paris             2     60"
      ]
     },
     "execution_count": 19,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "paris_listings.head()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 20,
   "id": "8f135166",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2025-02-10T03:28:17.731660Z",
     "iopub.status.busy": "2025-02-10T03:28:17.731447Z",
     "iopub.status.idle": "2025-02-10T03:28:17.744178Z",
     "shell.execute_reply": "2025-02-10T03:28:17.743425Z"
    },
    "papermill": {
     "duration": 0.024396,
     "end_time": "2025-02-10T03:28:17.745324",
     "exception": false,
     "start_time": "2025-02-10T03:28:17.720928",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "<class 'pandas.core.frame.DataFrame'>\n",
      "Index: 64690 entries, 0 to 279711\n",
      "Data columns (total 5 columns):\n",
      " #   Column         Non-Null Count  Dtype         \n",
      "---  ------         --------------  -----         \n",
      " 0   host_since     64657 non-null  datetime64[ns]\n",
      " 1   neighbourhood  64690 non-null  object        \n",
      " 2   city           64690 non-null  object        \n",
      " 3   accommodates   64690 non-null  int64         \n",
      " 4   price          64690 non-null  int64         \n",
      "dtypes: datetime64[ns](1), int64(2), object(2)\n",
      "memory usage: 3.0+ MB\n"
     ]
    }
   ],
   "source": [
    "paris_listings.info()"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "1d591ed9",
   "metadata": {
    "papermill": {
     "duration": 0.009831,
     "end_time": "2025-02-10T03:28:17.765332",
     "exception": false,
     "start_time": "2025-02-10T03:28:17.755501",
     "status": "completed"
    },
    "tags": []
   },
   "source": [
    "<div style=\"background-color: #f88379; padding: 25px; border-radius: 10px;\">\n",
    "  <h1 style=\"color: #333;\"><strong>Checking if there are missing values in the dataset</strong></h1>\n",
    "</div>"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 21,
   "id": "f37a39fa",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2025-02-10T03:28:17.786276Z",
     "iopub.status.busy": "2025-02-10T03:28:17.786076Z",
     "iopub.status.idle": "2025-02-10T03:28:17.796758Z",
     "shell.execute_reply": "2025-02-10T03:28:17.796067Z"
    },
    "papermill": {
     "duration": 0.022515,
     "end_time": "2025-02-10T03:28:17.798044",
     "exception": false,
     "start_time": "2025-02-10T03:28:17.775529",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "host_since       33\n",
       "neighbourhood     0\n",
       "city              0\n",
       "accommodates      0\n",
       "price             0\n",
       "dtype: int64"
      ]
     },
     "execution_count": 21,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "paris_listings.isna().sum()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 22,
   "id": "c6924766",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2025-02-10T03:28:17.819538Z",
     "iopub.status.busy": "2025-02-10T03:28:17.819281Z",
     "iopub.status.idle": "2025-02-10T03:28:17.830150Z",
     "shell.execute_reply": "2025-02-10T03:28:17.829586Z"
    },
    "papermill": {
     "duration": 0.022624,
     "end_time": "2025-02-10T03:28:17.831310",
     "exception": false,
     "start_time": "2025-02-10T03:28:17.808686",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [],
   "source": [
    "paris_listings.dropna(how = \"any\",inplace = True)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 23,
   "id": "48c77d1b",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2025-02-10T03:28:17.852487Z",
     "iopub.status.busy": "2025-02-10T03:28:17.852220Z",
     "iopub.status.idle": "2025-02-10T03:28:17.865514Z",
     "shell.execute_reply": "2025-02-10T03:28:17.864743Z"
    },
    "papermill": {
     "duration": 0.025067,
     "end_time": "2025-02-10T03:28:17.866693",
     "exception": false,
     "start_time": "2025-02-10T03:28:17.841626",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "<class 'pandas.core.frame.DataFrame'>\n",
      "Index: 64657 entries, 0 to 279711\n",
      "Data columns (total 5 columns):\n",
      " #   Column         Non-Null Count  Dtype         \n",
      "---  ------         --------------  -----         \n",
      " 0   host_since     64657 non-null  datetime64[ns]\n",
      " 1   neighbourhood  64657 non-null  object        \n",
      " 2   city           64657 non-null  object        \n",
      " 3   accommodates   64657 non-null  int64         \n",
      " 4   price          64657 non-null  int64         \n",
      "dtypes: datetime64[ns](1), int64(2), object(2)\n",
      "memory usage: 3.0+ MB\n"
     ]
    }
   ],
   "source": [
    "paris_listings.info()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 24,
   "id": "0960156d",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2025-02-10T03:28:17.888961Z",
     "iopub.status.busy": "2025-02-10T03:28:17.888751Z",
     "iopub.status.idle": "2025-02-10T03:28:17.909097Z",
     "shell.execute_reply": "2025-02-10T03:28:17.908427Z"
    },
    "papermill": {
     "duration": 0.032697,
     "end_time": "2025-02-10T03:28:17.910339",
     "exception": false,
     "start_time": "2025-02-10T03:28:17.877642",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>host_since</th>\n",
       "      <th>accommodates</th>\n",
       "      <th>price</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>count</th>\n",
       "      <td>64657</td>\n",
       "      <td>64657.000000</td>\n",
       "      <td>64657.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>mean</th>\n",
       "      <td>2015-11-01 11:06:05.528867584</td>\n",
       "      <td>3.037877</td>\n",
       "      <td>113.104614</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>min</th>\n",
       "      <td>2008-08-30 00:00:00</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>25%</th>\n",
       "      <td>2014-03-09 00:00:00</td>\n",
       "      <td>2.000000</td>\n",
       "      <td>59.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>50%</th>\n",
       "      <td>2015-07-07 00:00:00</td>\n",
       "      <td>2.000000</td>\n",
       "      <td>80.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>75%</th>\n",
       "      <td>2017-05-29 00:00:00</td>\n",
       "      <td>4.000000</td>\n",
       "      <td>120.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>max</th>\n",
       "      <td>2021-02-07 00:00:00</td>\n",
       "      <td>16.000000</td>\n",
       "      <td>12000.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>std</th>\n",
       "      <td>NaN</td>\n",
       "      <td>1.588382</td>\n",
       "      <td>214.479626</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "                          host_since  accommodates         price\n",
       "count                          64657  64657.000000  64657.000000\n",
       "mean   2015-11-01 11:06:05.528867584      3.037877    113.104614\n",
       "min              2008-08-30 00:00:00      0.000000      0.000000\n",
       "25%              2014-03-09 00:00:00      2.000000     59.000000\n",
       "50%              2015-07-07 00:00:00      2.000000     80.000000\n",
       "75%              2017-05-29 00:00:00      4.000000    120.000000\n",
       "max              2021-02-07 00:00:00     16.000000  12000.000000\n",
       "std                              NaN      1.588382    214.479626"
      ]
     },
     "execution_count": 24,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "paris_listings.describe()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 25,
   "id": "fa5818c0",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2025-02-10T03:28:17.932286Z",
     "iopub.status.busy": "2025-02-10T03:28:17.932083Z",
     "iopub.status.idle": "2025-02-10T03:28:17.952368Z",
     "shell.execute_reply": "2025-02-10T03:28:17.951744Z"
    },
    "papermill": {
     "duration": 0.032591,
     "end_time": "2025-02-10T03:28:17.953698",
     "exception": false,
     "start_time": "2025-02-10T03:28:17.921107",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>neighbourhood</th>\n",
       "      <th>city</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>count</th>\n",
       "      <td>64657</td>\n",
       "      <td>64657</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>unique</th>\n",
       "      <td>20</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>top</th>\n",
       "      <td>Buttes-Montmartre</td>\n",
       "      <td>Paris</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>freq</th>\n",
       "      <td>7232</td>\n",
       "      <td>64657</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "            neighbourhood   city\n",
       "count               64657  64657\n",
       "unique                 20      1\n",
       "top     Buttes-Montmartre  Paris\n",
       "freq                 7232  64657"
      ]
     },
     "execution_count": 25,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "paris_listings.describe(include=\"object\")"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 26,
   "id": "10034d16",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2025-02-10T03:28:17.975754Z",
     "iopub.status.busy": "2025-02-10T03:28:17.975554Z",
     "iopub.status.idle": "2025-02-10T03:28:17.981454Z",
     "shell.execute_reply": "2025-02-10T03:28:17.980801Z"
    },
    "papermill": {
     "duration": 0.018446,
     "end_time": "2025-02-10T03:28:17.982760",
     "exception": false,
     "start_time": "2025-02-10T03:28:17.964314",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "host_since       54\n",
       "neighbourhood    54\n",
       "city             54\n",
       "accommodates     54\n",
       "price            54\n",
       "dtype: int64"
      ]
     },
     "execution_count": 26,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "paris_listings[paris_listings[\"accommodates\"]==0].count(axis = 0)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 27,
   "id": "1be86c67",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2025-02-10T03:28:18.004780Z",
     "iopub.status.busy": "2025-02-10T03:28:18.004582Z",
     "iopub.status.idle": "2025-02-10T03:28:18.010168Z",
     "shell.execute_reply": "2025-02-10T03:28:18.009556Z"
    },
    "papermill": {
     "duration": 0.017874,
     "end_time": "2025-02-10T03:28:18.011310",
     "exception": false,
     "start_time": "2025-02-10T03:28:17.993436",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "host_since       62\n",
       "neighbourhood    62\n",
       "city             62\n",
       "accommodates     62\n",
       "price            62\n",
       "dtype: int64"
      ]
     },
     "execution_count": 27,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "paris_listings[paris_listings[\"price\"]==0].count(axis = 0)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 28,
   "id": "4e0bc6cb",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2025-02-10T03:28:18.033250Z",
     "iopub.status.busy": "2025-02-10T03:28:18.033056Z",
     "iopub.status.idle": "2025-02-10T03:28:18.041476Z",
     "shell.execute_reply": "2025-02-10T03:28:18.040742Z"
    },
    "papermill": {
     "duration": 0.020871,
     "end_time": "2025-02-10T03:28:18.042813",
     "exception": false,
     "start_time": "2025-02-10T03:28:18.021942",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>host_since</th>\n",
       "      <th>neighbourhood</th>\n",
       "      <th>city</th>\n",
       "      <th>accommodates</th>\n",
       "      <th>price</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>207075</th>\n",
       "      <td>2019-07-22</td>\n",
       "      <td>Vaugirard</td>\n",
       "      <td>Paris</td>\n",
       "      <td>2</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>207076</th>\n",
       "      <td>2019-10-31</td>\n",
       "      <td>Enclos-St-Laurent</td>\n",
       "      <td>Paris</td>\n",
       "      <td>2</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>208301</th>\n",
       "      <td>2019-05-27</td>\n",
       "      <td>Palais-Bourbon</td>\n",
       "      <td>Paris</td>\n",
       "      <td>2</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>208302</th>\n",
       "      <td>2019-07-19</td>\n",
       "      <td>Pantheon</td>\n",
       "      <td>Paris</td>\n",
       "      <td>3</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>208854</th>\n",
       "      <td>2016-09-16</td>\n",
       "      <td>Louvre</td>\n",
       "      <td>Paris</td>\n",
       "      <td>3</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>208855</th>\n",
       "      <td>2019-11-06</td>\n",
       "      <td>Luxembourg</td>\n",
       "      <td>Paris</td>\n",
       "      <td>3</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>208856</th>\n",
       "      <td>2019-12-02</td>\n",
       "      <td>Elysee</td>\n",
       "      <td>Paris</td>\n",
       "      <td>3</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>208857</th>\n",
       "      <td>2019-12-17</td>\n",
       "      <td>Opera</td>\n",
       "      <td>Paris</td>\n",
       "      <td>2</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "       host_since      neighbourhood   city  accommodates  price\n",
       "207075 2019-07-22          Vaugirard  Paris             2      0\n",
       "207076 2019-10-31  Enclos-St-Laurent  Paris             2      0\n",
       "208301 2019-05-27     Palais-Bourbon  Paris             2      0\n",
       "208302 2019-07-19           Pantheon  Paris             3      0\n",
       "208854 2016-09-16             Louvre  Paris             3      0\n",
       "208855 2019-11-06         Luxembourg  Paris             3      0\n",
       "208856 2019-12-02             Elysee  Paris             3      0\n",
       "208857 2019-12-17              Opera  Paris             2      0"
      ]
     },
     "execution_count": 28,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "paris_listings[(paris_listings[\"price\"]==0) & (paris_listings[\"accommodates\"]!=0)]"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 29,
   "id": "1047ae90",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2025-02-10T03:28:18.065418Z",
     "iopub.status.busy": "2025-02-10T03:28:18.065202Z",
     "iopub.status.idle": "2025-02-10T03:28:18.070377Z",
     "shell.execute_reply": "2025-02-10T03:28:18.069644Z"
    },
    "papermill": {
     "duration": 0.017605,
     "end_time": "2025-02-10T03:28:18.071697",
     "exception": false,
     "start_time": "2025-02-10T03:28:18.054092",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [],
   "source": [
    "paris_listings = paris_listings[paris_listings[\"price\"]!=0]"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 30,
   "id": "4e0f1c22",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2025-02-10T03:28:18.093891Z",
     "iopub.status.busy": "2025-02-10T03:28:18.093694Z",
     "iopub.status.idle": "2025-02-10T03:28:18.106256Z",
     "shell.execute_reply": "2025-02-10T03:28:18.105454Z"
    },
    "papermill": {
     "duration": 0.024855,
     "end_time": "2025-02-10T03:28:18.107464",
     "exception": false,
     "start_time": "2025-02-10T03:28:18.082609",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "<class 'pandas.core.frame.DataFrame'>\n",
      "Index: 64595 entries, 0 to 279711\n",
      "Data columns (total 5 columns):\n",
      " #   Column         Non-Null Count  Dtype         \n",
      "---  ------         --------------  -----         \n",
      " 0   host_since     64595 non-null  datetime64[ns]\n",
      " 1   neighbourhood  64595 non-null  object        \n",
      " 2   city           64595 non-null  object        \n",
      " 3   accommodates   64595 non-null  int64         \n",
      " 4   price          64595 non-null  int64         \n",
      "dtypes: datetime64[ns](1), int64(2), object(2)\n",
      "memory usage: 3.0+ MB\n"
     ]
    }
   ],
   "source": [
    "paris_listings.info()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 31,
   "id": "fd19e4e2",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2025-02-10T03:28:18.129918Z",
     "iopub.status.busy": "2025-02-10T03:28:18.129706Z",
     "iopub.status.idle": "2025-02-10T03:28:18.136663Z",
     "shell.execute_reply": "2025-02-10T03:28:18.135965Z"
    },
    "papermill": {
     "duration": 0.019611,
     "end_time": "2025-02-10T03:28:18.137939",
     "exception": false,
     "start_time": "2025-02-10T03:28:18.118328",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>host_since</th>\n",
       "      <th>neighbourhood</th>\n",
       "      <th>city</th>\n",
       "      <th>accommodates</th>\n",
       "      <th>price</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>2011-12-03</td>\n",
       "      <td>Buttes-Montmartre</td>\n",
       "      <td>Paris</td>\n",
       "      <td>2</td>\n",
       "      <td>53</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>2013-11-29</td>\n",
       "      <td>Buttes-Montmartre</td>\n",
       "      <td>Paris</td>\n",
       "      <td>2</td>\n",
       "      <td>120</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>2014-07-31</td>\n",
       "      <td>Elysee</td>\n",
       "      <td>Paris</td>\n",
       "      <td>2</td>\n",
       "      <td>89</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>2013-12-17</td>\n",
       "      <td>Vaugirard</td>\n",
       "      <td>Paris</td>\n",
       "      <td>2</td>\n",
       "      <td>58</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>2014-12-14</td>\n",
       "      <td>Passy</td>\n",
       "      <td>Paris</td>\n",
       "      <td>2</td>\n",
       "      <td>60</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "  host_since      neighbourhood   city  accommodates  price\n",
       "0 2011-12-03  Buttes-Montmartre  Paris             2     53\n",
       "1 2013-11-29  Buttes-Montmartre  Paris             2    120\n",
       "2 2014-07-31             Elysee  Paris             2     89\n",
       "3 2013-12-17          Vaugirard  Paris             2     58\n",
       "4 2014-12-14              Passy  Paris             2     60"
      ]
     },
     "execution_count": 31,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "paris_listings.head()"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "a93e9954",
   "metadata": {
    "papermill": {
     "duration": 0.010842,
     "end_time": "2025-02-10T03:28:18.160031",
     "exception": false,
     "start_time": "2025-02-10T03:28:18.149189",
     "status": "completed"
    },
    "tags": []
   },
   "source": [
    "<div style=\"background-color: #f88379; padding: 25px; border-radius: 10px;\">\n",
    "  <h1 style=\"color: #333;\"><strong>Finding average Price of AirBnB in Paris as per Localities</strong></h1>\n",
    "</div>"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 32,
   "id": "4b188a48",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2025-02-10T03:28:18.182830Z",
     "iopub.status.busy": "2025-02-10T03:28:18.182612Z",
     "iopub.status.idle": "2025-02-10T03:28:18.193225Z",
     "shell.execute_reply": "2025-02-10T03:28:18.192424Z"
    },
    "papermill": {
     "duration": 0.023367,
     "end_time": "2025-02-10T03:28:18.194564",
     "exception": false,
     "start_time": "2025-02-10T03:28:18.171197",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [],
   "source": [
    "paris_listings_neighbourhood = paris_listings.groupby(\"neighbourhood\").agg({\"price\":\"mean\"}).sort_values(\"price\",ascending=False).round(2)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 33,
   "id": "15524630",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2025-02-10T03:28:18.216906Z",
     "iopub.status.busy": "2025-02-10T03:28:18.216701Z",
     "iopub.status.idle": "2025-02-10T03:28:18.223526Z",
     "shell.execute_reply": "2025-02-10T03:28:18.222876Z"
    },
    "papermill": {
     "duration": 0.019183,
     "end_time": "2025-02-10T03:28:18.224634",
     "exception": false,
     "start_time": "2025-02-10T03:28:18.205451",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>price</th>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>neighbourhood</th>\n",
       "      <th></th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>Elysee</th>\n",
       "      <td>211.37</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Louvre</th>\n",
       "      <td>175.75</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Passy</th>\n",
       "      <td>161.29</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Palais-Bourbon</th>\n",
       "      <td>157.51</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Luxembourg</th>\n",
       "      <td>155.79</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Bourse</th>\n",
       "      <td>149.63</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Hotel-de-Ville</th>\n",
       "      <td>144.52</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Temple</th>\n",
       "      <td>138.43</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Pantheon</th>\n",
       "      <td>122.98</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Opera</th>\n",
       "      <td>119.20</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Vaugirard</th>\n",
       "      <td>107.09</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Enclos-St-Laurent</th>\n",
       "      <td>103.12</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Batignolles-Monceau</th>\n",
       "      <td>102.69</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Observatoire</th>\n",
       "      <td>102.00</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Gobelins</th>\n",
       "      <td>98.11</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Popincourt</th>\n",
       "      <td>90.55</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Reuilly</th>\n",
       "      <td>89.13</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Buttes-Montmartre</th>\n",
       "      <td>87.25</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Buttes-Chaumont</th>\n",
       "      <td>82.69</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Menilmontant</th>\n",
       "      <td>74.93</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "                      price\n",
       "neighbourhood              \n",
       "Elysee               211.37\n",
       "Louvre               175.75\n",
       "Passy                161.29\n",
       "Palais-Bourbon       157.51\n",
       "Luxembourg           155.79\n",
       "Bourse               149.63\n",
       "Hotel-de-Ville       144.52\n",
       "Temple               138.43\n",
       "Pantheon             122.98\n",
       "Opera                119.20\n",
       "Vaugirard            107.09\n",
       "Enclos-St-Laurent    103.12\n",
       "Batignolles-Monceau  102.69\n",
       "Observatoire         102.00\n",
       "Gobelins              98.11\n",
       "Popincourt            90.55\n",
       "Reuilly               89.13\n",
       "Buttes-Montmartre     87.25\n",
       "Buttes-Chaumont       82.69\n",
       "Menilmontant          74.93"
      ]
     },
     "execution_count": 33,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "paris_listings_neighbourhood"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 34,
   "id": "3bcc3c6f",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2025-02-10T03:28:18.247561Z",
     "iopub.status.busy": "2025-02-10T03:28:18.247326Z",
     "iopub.status.idle": "2025-02-10T03:28:18.256268Z",
     "shell.execute_reply": "2025-02-10T03:28:18.255548Z"
    },
    "papermill": {
     "duration": 0.021864,
     "end_time": "2025-02-10T03:28:18.257563",
     "exception": false,
     "start_time": "2025-02-10T03:28:18.235699",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [],
   "source": [
    "paris_Elysee_accommodates = paris_listings.query(\"neighbourhood=='Elysee'\").groupby(\"accommodates\").agg({\"price\":\"mean\"}).sort_values(\"price\",ascending=False).round(2)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 35,
   "id": "7288f8f1",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2025-02-10T03:28:18.280524Z",
     "iopub.status.busy": "2025-02-10T03:28:18.280293Z",
     "iopub.status.idle": "2025-02-10T03:28:18.282995Z",
     "shell.execute_reply": "2025-02-10T03:28:18.282384Z"
    },
    "papermill": {
     "duration": 0.015421,
     "end_time": "2025-02-10T03:28:18.284150",
     "exception": false,
     "start_time": "2025-02-10T03:28:18.268729",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [],
   "source": [
    "# Inside of Paris,the price of AirBnB is directly impacted by the popularity of the tourist place or\n",
    "# the view of Eiffel tower. We need to further see if the prices are increasing when the regulations are imposed"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 36,
   "id": "2d4e1918",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2025-02-10T03:28:18.307181Z",
     "iopub.status.busy": "2025-02-10T03:28:18.306990Z",
     "iopub.status.idle": "2025-02-10T03:28:18.313431Z",
     "shell.execute_reply": "2025-02-10T03:28:18.312807Z"
    },
    "papermill": {
     "duration": 0.019176,
     "end_time": "2025-02-10T03:28:18.314613",
     "exception": false,
     "start_time": "2025-02-10T03:28:18.295437",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>price</th>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>accommodates</th>\n",
       "      <th></th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>14</th>\n",
       "      <td>971.00</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>13</th>\n",
       "      <td>842.50</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>11</th>\n",
       "      <td>805.00</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>16</th>\n",
       "      <td>800.00</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>12</th>\n",
       "      <td>529.62</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>10</th>\n",
       "      <td>500.86</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>9</th>\n",
       "      <td>440.27</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>7</th>\n",
       "      <td>411.54</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>8</th>\n",
       "      <td>405.52</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>6</th>\n",
       "      <td>355.51</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>5</th>\n",
       "      <td>328.82</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>212.10</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>155.10</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>153.88</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>79.52</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "               price\n",
       "accommodates        \n",
       "14            971.00\n",
       "13            842.50\n",
       "11            805.00\n",
       "16            800.00\n",
       "12            529.62\n",
       "10            500.86\n",
       "9             440.27\n",
       "7             411.54\n",
       "8             405.52\n",
       "6             355.51\n",
       "5             328.82\n",
       "4             212.10\n",
       "2             155.10\n",
       "3             153.88\n",
       "1              79.52"
      ]
     },
     "execution_count": 36,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "paris_Elysee_accommodates"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 37,
   "id": "7f8d7f42",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2025-02-10T03:28:18.337767Z",
     "iopub.status.busy": "2025-02-10T03:28:18.337573Z",
     "iopub.status.idle": "2025-02-10T03:28:18.339975Z",
     "shell.execute_reply": "2025-02-10T03:28:18.339421Z"
    },
    "papermill": {
     "duration": 0.015171,
     "end_time": "2025-02-10T03:28:18.341120",
     "exception": false,
     "start_time": "2025-02-10T03:28:18.325949",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [],
   "source": [
    "# The AirBnB with 14 people accommodates is the expensive, even more expensive the 16 accommodates\n",
    "# the same trend is spotted for 11 and 12 accommodates. Every other accommodation looks reasonable."
   ]
  },
  {
   "cell_type": "markdown",
   "id": "64189f75",
   "metadata": {
    "papermill": {
     "duration": 0.011024,
     "end_time": "2025-02-10T03:28:18.363600",
     "exception": false,
     "start_time": "2025-02-10T03:28:18.352576",
     "status": "completed"
    },
    "tags": []
   },
   "source": [
    "### Possible reasons for this trend\n",
    "1. Owners of properties with 16 people might face occupancy issues due to less number of large groups.\n",
    "2. The ones with 14 people might be a sweet spot for a group having two families travelling.\n",
    "3. There is possibility that more than 16 and 12 occupancy properties are away from hotspots in Elysee.\n",
    "4. There might be potential outliers in 11 and 14 occupancy properties which are skewing the trend."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 38,
   "id": "6b9622ca",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2025-02-10T03:28:18.386547Z",
     "iopub.status.busy": "2025-02-10T03:28:18.386306Z",
     "iopub.status.idle": "2025-02-10T03:28:18.392651Z",
     "shell.execute_reply": "2025-02-10T03:28:18.392037Z"
    },
    "papermill": {
     "duration": 0.019161,
     "end_time": "2025-02-10T03:28:18.393839",
     "exception": false,
     "start_time": "2025-02-10T03:28:18.374678",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "3854"
      ]
     },
     "execution_count": 38,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "paris_listings[\"host_since\"].nunique()"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "f99ec7b0",
   "metadata": {
    "papermill": {
     "duration": 0.01159,
     "end_time": "2025-02-10T03:28:18.416724",
     "exception": false,
     "start_time": "2025-02-10T03:28:18.405134",
     "status": "completed"
    },
    "tags": []
   },
   "source": [
    "<div style=\"background-color: #f88379; padding: 25px; border-radius: 10px;\">\n",
    "  <h1 style=\"color: #333;\"><strong>Finding popularity of AirBnB over time</strong></h1>\n",
    "</div>"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 39,
   "id": "96c43a4b",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2025-02-10T03:28:18.448794Z",
     "iopub.status.busy": "2025-02-10T03:28:18.448410Z",
     "iopub.status.idle": "2025-02-10T03:28:18.487568Z",
     "shell.execute_reply": "2025-02-10T03:28:18.486721Z"
    },
    "papermill": {
     "duration": 0.05483,
     "end_time": "2025-02-10T03:28:18.489367",
     "exception": false,
     "start_time": "2025-02-10T03:28:18.434537",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [],
   "source": [
    "paris_listings_over_time = paris_listings.set_index(\"host_since\").resample(\"Y\").agg({\"neighbourhood\":\"count\",\"price\":\"mean\"})"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 40,
   "id": "a291113a",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2025-02-10T03:28:18.514773Z",
     "iopub.status.busy": "2025-02-10T03:28:18.514419Z",
     "iopub.status.idle": "2025-02-10T03:28:18.523945Z",
     "shell.execute_reply": "2025-02-10T03:28:18.523121Z"
    },
    "papermill": {
     "duration": 0.023654,
     "end_time": "2025-02-10T03:28:18.525581",
     "exception": false,
     "start_time": "2025-02-10T03:28:18.501927",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>neighbourhood</th>\n",
       "      <th>price</th>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>host_since</th>\n",
       "      <th></th>\n",
       "      <th></th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>2008-12-31</th>\n",
       "      <td>4</td>\n",
       "      <td>77.750000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2009-12-31</th>\n",
       "      <td>106</td>\n",
       "      <td>159.641509</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2010-12-31</th>\n",
       "      <td>416</td>\n",
       "      <td>125.031250</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2011-12-31</th>\n",
       "      <td>1339</td>\n",
       "      <td>124.828230</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2012-12-31</th>\n",
       "      <td>4592</td>\n",
       "      <td>111.578615</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2013-12-31</th>\n",
       "      <td>8142</td>\n",
       "      <td>107.096414</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2014-12-31</th>\n",
       "      <td>10922</td>\n",
       "      <td>100.253800</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2015-12-31</th>\n",
       "      <td>12147</td>\n",
       "      <td>103.646250</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2016-12-31</th>\n",
       "      <td>8867</td>\n",
       "      <td>114.211345</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2017-12-31</th>\n",
       "      <td>4585</td>\n",
       "      <td>108.658888</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2018-12-31</th>\n",
       "      <td>4294</td>\n",
       "      <td>138.209362</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2019-12-31</th>\n",
       "      <td>5685</td>\n",
       "      <td>129.962533</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2020-12-31</th>\n",
       "      <td>3363</td>\n",
       "      <td>143.517098</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2021-12-31</th>\n",
       "      <td>133</td>\n",
       "      <td>93.488722</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "            neighbourhood       price\n",
       "host_since                           \n",
       "2008-12-31              4   77.750000\n",
       "2009-12-31            106  159.641509\n",
       "2010-12-31            416  125.031250\n",
       "2011-12-31           1339  124.828230\n",
       "2012-12-31           4592  111.578615\n",
       "2013-12-31           8142  107.096414\n",
       "2014-12-31          10922  100.253800\n",
       "2015-12-31          12147  103.646250\n",
       "2016-12-31           8867  114.211345\n",
       "2017-12-31           4585  108.658888\n",
       "2018-12-31           4294  138.209362\n",
       "2019-12-31           5685  129.962533\n",
       "2020-12-31           3363  143.517098\n",
       "2021-12-31            133   93.488722"
      ]
     },
     "execution_count": 40,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "paris_listings_over_time"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 41,
   "id": "e2dc255b",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2025-02-10T03:28:18.556983Z",
     "iopub.status.busy": "2025-02-10T03:28:18.556746Z",
     "iopub.status.idle": "2025-02-10T03:28:19.908447Z",
     "shell.execute_reply": "2025-02-10T03:28:19.907572Z"
    },
    "papermill": {
     "duration": 1.365739,
     "end_time": "2025-02-10T03:28:19.910135",
     "exception": false,
     "start_time": "2025-02-10T03:28:18.544396",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [],
   "source": [
    "import matplotlib.pyplot as plt\n",
    "import seaborn as sns"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 42,
   "id": "e67bd4b5",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2025-02-10T03:28:19.935679Z",
     "iopub.status.busy": "2025-02-10T03:28:19.935273Z",
     "iopub.status.idle": "2025-02-10T03:28:19.943339Z",
     "shell.execute_reply": "2025-02-10T03:28:19.942544Z"
    },
    "papermill": {
     "duration": 0.021851,
     "end_time": "2025-02-10T03:28:19.944652",
     "exception": false,
     "start_time": "2025-02-10T03:28:19.922801",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>price</th>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>neighbourhood</th>\n",
       "      <th></th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>Elysee</th>\n",
       "      <td>211.37</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Louvre</th>\n",
       "      <td>175.75</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Passy</th>\n",
       "      <td>161.29</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Palais-Bourbon</th>\n",
       "      <td>157.51</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Luxembourg</th>\n",
       "      <td>155.79</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Bourse</th>\n",
       "      <td>149.63</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Hotel-de-Ville</th>\n",
       "      <td>144.52</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Temple</th>\n",
       "      <td>138.43</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Pantheon</th>\n",
       "      <td>122.98</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Opera</th>\n",
       "      <td>119.20</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Vaugirard</th>\n",
       "      <td>107.09</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Enclos-St-Laurent</th>\n",
       "      <td>103.12</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Batignolles-Monceau</th>\n",
       "      <td>102.69</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Observatoire</th>\n",
       "      <td>102.00</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Gobelins</th>\n",
       "      <td>98.11</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Popincourt</th>\n",
       "      <td>90.55</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Reuilly</th>\n",
       "      <td>89.13</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Buttes-Montmartre</th>\n",
       "      <td>87.25</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Buttes-Chaumont</th>\n",
       "      <td>82.69</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Menilmontant</th>\n",
       "      <td>74.93</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "                      price\n",
       "neighbourhood              \n",
       "Elysee               211.37\n",
       "Louvre               175.75\n",
       "Passy                161.29\n",
       "Palais-Bourbon       157.51\n",
       "Luxembourg           155.79\n",
       "Bourse               149.63\n",
       "Hotel-de-Ville       144.52\n",
       "Temple               138.43\n",
       "Pantheon             122.98\n",
       "Opera                119.20\n",
       "Vaugirard            107.09\n",
       "Enclos-St-Laurent    103.12\n",
       "Batignolles-Monceau  102.69\n",
       "Observatoire         102.00\n",
       "Gobelins              98.11\n",
       "Popincourt            90.55\n",
       "Reuilly               89.13\n",
       "Buttes-Montmartre     87.25\n",
       "Buttes-Chaumont       82.69\n",
       "Menilmontant          74.93"
      ]
     },
     "execution_count": 42,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "paris_listings_neighbourhood"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 43,
   "id": "0a709f6a",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2025-02-10T03:28:19.969052Z",
     "iopub.status.busy": "2025-02-10T03:28:19.968804Z",
     "iopub.status.idle": "2025-02-10T03:28:20.330675Z",
     "shell.execute_reply": "2025-02-10T03:28:20.329833Z"
    },
    "papermill": {
     "duration": 0.375541,
     "end_time": "2025-02-10T03:28:20.332040",
     "exception": false,
     "start_time": "2025-02-10T03:28:19.956499",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [
    {
     "data": {
      "image/png": "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vXt3Pf300+rRo4ckyd/f36JORr4HHvbfkQcxDEMvvPCCNm7cqG7duqlSpUpas2aN3nrrLf3111/me0zx008/acGCBerbt68cHR01depUhYaG6rfffsvwXEZVqlSRdHdOqeDg4IeKG8BjwgAAwIZmz55tSDK2b99uHD9+3LC3tzf69u1rPh4SEmKUK1fOvH/q1CkjV65cxrhx4yzOs2/fPsPe3t6ivHPnzoavr695f/HixYYkY/LkyeaypKQko0GDBoYkY/bs2RZtJRmjR4+2uE5wcLBRpUoV8/7JkycNSYazs7Px559/mst//fVXQ5Lx5ptvmssqVapkFChQwLh06ZK5bM+ePYadnZ3RqVOndONOMXLkSOPef6olGXZ2dsbvv/9uUZ7yXBs2bGgkJyeby998800jV65cxtWrVw3DMIzz588bDg4OxnPPPWckJSWZ602ZMsWQZMyaNcswDMNITEw0ChQoYJQvX964efOmud7y5csNScaIESMs7rNw4cLmaxiGYaxdu9aQZHFf/fr1Mzw8PIw7d+6kutcHkWSMHDky1bPp2rWrRb2WLVsa+fLle+D5OnfubLi6uhoXLlwwLly4YBw7dswYP368YTKZjAoVKhiGYRi3bt2yeEaGcff1d3R0tHifbNy40ZBklChRwrhx44ZF/ZRjGzduNAzDMHbv3m1IMhYuXGjN7RuGYRi1a9e2eC/ee41Zs2YZFy5cMP7++29jxYoVhp+fn2EymYzt27cbhvF/z6xdu3YW7a35jIWEhBiSjK+//tpcdujQIfP7ctu2bebyNWvWpPqcpbxPT548aS6797VN4evra3Tu3DlV28aNG1u8x2vUqGGYTCajZ8+e5rI7d+4YTz31lBESEpLqvPe69zvnXhn9zNy5c8coXry44evra1y5csXiHP+O9973iGEYxjfffGNIMn7++edU9/vvZxUSEmJxT9u3b0/1jFPc+73yyy+/GJKMefPmWdRbvXq1RfnSpUvN39HWSuveXnvtNcPFxcW4deuWuez5559P8zsvPa6urhbvhRQZ/R6w5j3+IL1797b4Xl62bJkhyRg7dqxFvZdeeskwmUzGsWPHzGWSDEnGjh07zGV//PGH4eTkZLRs2dKifVqv/785ODgYvXr1ynDcAB5PDFkCAGSbEiVKqGPHjpoxY4bOnj2bZp0lS5YoOTlZrVu31sWL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U1599VW9+uqrCgkJ0ebNm/Xll1+qbNmykqSKFSsqMTFRV69eVevWrRUcHKyhQ4fKy8tLdnb5+6rn5eWld999V6Ghoapdu7bWrVunr776SmXKlLnpPhERETpw4IB8fX3zJKYmT56scePGKTY2VkFBQQoPD9eqVatUpUoVq3apqalWr+e+mTJlyuiJJ54w1635s5tdX+naSJc5c+bo7bffVkhIiLZv337b9WokycXFRWvWrNHZs2fVoEEDPf3003rsscfMt1+5uLjo4MGDeuqpp1S9enX169dPUVFR6t+/v9nH4sWL1bdv39seCwBw/7IYdzr2EwAAADZx7NgxValSRd9//73q1KlT2OEUqr1796pVq1ZKTU2Vm5tbYYdzS/v371eLFi106NAheXp6FnY4AIAiihEyAAAAKPJq166t1157TUePHi3sUG4rPT1dCxcuJBkDALglRsgAAAAUUYyQAQCg+CIhAwAAAAAAYGNMWQIAAAAAALAxEjIAAAAAAAA2RkIGAAAAAADAxkjIAAAAAAAA2BgJGQAAAAAAABsjIQMAAAAAAGBjJGQAAAAAAABsjIQMAAAAAACAjf1/kCy5PVLApt8AAAAASUVORK5CYII=\n",
      "text/plain": [
       "<Figure size 1200x800 with 1 Axes>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "plt.figure(figsize = (12,8))\n",
    "sns.barplot(data = paris_listings_neighbourhood,\n",
    "           x = \"price\",\n",
    "           y = paris_listings_neighbourhood.index,\n",
    "           color = \"#E5989B\")\n",
    "plt.title(\"Neighbourhoods in Paris (Premium Localities at the Top!)\")\n",
    "plt.xlabel(\"How Expensive? (Euros)\")\n",
    "plt.ylabel(\"Prominent Places with AirBnBs\")\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 44,
   "id": "9ad7d24c",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2025-02-10T03:28:20.358808Z",
     "iopub.status.busy": "2025-02-10T03:28:20.358556Z",
     "iopub.status.idle": "2025-02-10T03:28:20.361481Z",
     "shell.execute_reply": "2025-02-10T03:28:20.360721Z"
    },
    "papermill": {
     "duration": 0.017569,
     "end_time": "2025-02-10T03:28:20.362856",
     "exception": false,
     "start_time": "2025-02-10T03:28:20.345287",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [],
   "source": [
    "# Inside of Paris, the price of AirBnBs is directly impacted by the popularity of the tourist place or \n",
    "# the view of Eiffel tower. We need to further see if the prices are increasing when the regulations are imposed."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 45,
   "id": "c4e95e3f",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2025-02-10T03:28:20.389269Z",
     "iopub.status.busy": "2025-02-10T03:28:20.389049Z",
     "iopub.status.idle": "2025-02-10T03:28:20.396328Z",
     "shell.execute_reply": "2025-02-10T03:28:20.395595Z"
    },
    "papermill": {
     "duration": 0.021791,
     "end_time": "2025-02-10T03:28:20.397542",
     "exception": false,
     "start_time": "2025-02-10T03:28:20.375751",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>price</th>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>accommodates</th>\n",
       "      <th></th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>14</th>\n",
       "      <td>971.00</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>13</th>\n",
       "      <td>842.50</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>11</th>\n",
       "      <td>805.00</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>16</th>\n",
       "      <td>800.00</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>12</th>\n",
       "      <td>529.62</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>10</th>\n",
       "      <td>500.86</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>9</th>\n",
       "      <td>440.27</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>7</th>\n",
       "      <td>411.54</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>8</th>\n",
       "      <td>405.52</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>6</th>\n",
       "      <td>355.51</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>5</th>\n",
       "      <td>328.82</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>212.10</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>155.10</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>153.88</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>79.52</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "               price\n",
       "accommodates        \n",
       "14            971.00\n",
       "13            842.50\n",
       "11            805.00\n",
       "16            800.00\n",
       "12            529.62\n",
       "10            500.86\n",
       "9             440.27\n",
       "7             411.54\n",
       "8             405.52\n",
       "6             355.51\n",
       "5             328.82\n",
       "4             212.10\n",
       "2             155.10\n",
       "3             153.88\n",
       "1              79.52"
      ]
     },
     "execution_count": 45,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "paris_Elysee_accommodates"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 46,
   "id": "e0b3d73c",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2025-02-10T03:28:20.423929Z",
     "iopub.status.busy": "2025-02-10T03:28:20.423728Z",
     "iopub.status.idle": "2025-02-10T03:28:20.649412Z",
     "shell.execute_reply": "2025-02-10T03:28:20.648661Z"
    },
    "papermill": {
     "duration": 0.240392,
     "end_time": "2025-02-10T03:28:20.650854",
     "exception": false,
     "start_time": "2025-02-10T03:28:20.410462",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [
    {
     "data": {
      "image/png": "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yKn9Wsfbzz//rAYNGihfvnwqU6aM43Nxw4YNqlWrluNzZPXq1enu89SpU+rcubMCAwNVuHBhDRo0SBcuXEjT79NPP1XNmjXl5+enQoUKqWvXrmkuIcnKz/LatWtVr149+fv7q0CBAmrbtq327NnjWN67d2/Hl3udOnWSzWZTw4YNr7nN9Nx3332S5Ph8lqTly5c79h0QEKBWrVrp119/dblG6f8+f1I/w6/3fl4tsz93V4uIiNCvv/6qDRs2OD6vr35/kpKSNHToUBUtWlT+/v5q3769/vnnH6c+X375pVq1aqWQkBDZ7XZFRkZq7NixSk5OdvRp2LChvv76ax0+fNixr4iIiOu+tk8//VR333238uXLp4IFC6p+/fr65ptvXNp36v5v5DN21apVqlu3rgoUKKD8+fOrfPnyjuNz/fr1uuuuuyT99yXO1b/7vv32W3Xq1ElhYWGOv2OGDBmi8+fPO7Y/a9Ys2Ww27dixI82+X3vtNXl6euro0aOZqhXITRgpBywUHR2tDh06yMfHR926ddO0adO0ZcsW3XXXXfL29lb79u21aNEivf/++06jqUuWLFFSUpK6du0q6b8/3Nu0aaPvvvtO/fr1U8WKFbV7925NnjxZ+/bt05IlS5z2u3btWi1YsEADBgxQkSJFHL/w33rrLbVp00bdu3fXxYsXNW/ePHXq1EnLli1Tq1atHOv37t1bCxYsUI8ePXTPPfdow4YNTstT/f3337rnnnscXwQULVpUy5cv1yOPPKL4+PgsnZp36dIlHTlyRIULF3ZqnzVrli5cuKB+/frJbrerUKFC6f6h9fPPP6tevXry9vZWv379FBERoT/++ENLly7VuHHj3FL3o48+qjlz5qhjx4565pln9NNPPykqKkp79uxxBIZPPvlEM2bM0ObNm/Xhhx9KkmrXrn3N7R47dkzr1q3TnDlzJEndunXT5MmT9e6772ZqtP3ff/9Vy5Yt1bVrVz388MMqVqyYY1lycrJjMqpLly5pz549ji956tSpk2ZbnTt3VqlSpRQVFaXt27frww8/VHBwsCZMmODU79KlS47tXrhwQTt27NCkSZNUv359lSpV6ro1pyc2NlYtWrRQhw4d1LlzZ33xxRd64YUXVKVKFbVs2fK6619Z05X8/f3l5+eX7jo9evTQY489pl9++UW33367o33Lli3at2+fXn75ZUn//UHarVs3NW7c2PFe7NmzR5s2bdKgQYMk/Teq2aBBAx09elSPP/64wsLC9P3332v48OE6fvy407Wkjz/+uGbPnq0+ffpo4MCBOnjwoN59913t2LFDmzZtytZLMzp37qznn39eCxYs0HPPPee0bMGCBWrWrJkKFiyoixcvqnnz5kpKStLTTz+t4sWL6+jRo1q2bJnOnDmjoKCgbKvxSg899JDGjh2rMWPGqH379m4bLY+NjdUDDzygrl27qlOnTpo2bZq6du2q6OhoDR48WE888YQeeughvf766+rYsaOOHDmigIAAp2107txZERERioqK0o8//qi3335bsbGx+vjjjx19xo0bpxEjRqhz58569NFH9c8//+idd95R/fr1tWPHDqcR7Gv9LF9t9erVatmypUqXLq1Ro0bp/Pnzeuedd1SnTh1t375dERERevzxx3Xbbbfptddec5ySfq1tZuSPP/6QJMfn8yeffKJevXqpefPmmjBhgs6dO6dp06apbt262rFjh+N3T2ZqdPX9vJorP3dXmzJlip5++mnlz5/f8QXu1e/P008/rYIFC2rkyJE6dOiQpkyZogEDBmj+/PmOPrNnz1b+/Pk1dOhQ5c+fX2vXrtUrr7yi+Ph4vf7665Kkl156SXFxcfrrr780efJkSVL+/Pmv+b6PHj1ao0aNUu3atTVmzBj5+Pjop59+0tq1a9WsWbNM7ztVVj9jf/31Vz3wwAOqWrWqxowZI7vdrgMHDmjTpk2SpIoVK2rMmDF65ZVX1K9fP9WrV0/S//3u+/zzz3Xu3Dk9+eSTKly4sDZv3qx33nlHf/31lz7//HNJUseOHdW/f39FR0erevXqTvuPjo5Ww4YNddttt13z/QJyJQPAElu3bjWSzKpVq4wxxqSkpJiSJUuaQYMGOfqsXLnSSDJLly51Wvf+++83pUuXdjz/5JNPjIeHh/n222+d+k2fPt1IMps2bXK0STIeHh7m119/TVPTuXPnnJ5fvHjR3H777ea+++5ztG3bts1IMoMHD3bq27t3byPJjBw50tH2yCOPmBIlSphTp0459e3atasJCgpKs7+rhYeHm2bNmpl//vnH/PPPP2bXrl2ma9euRpJ5+umnjTHGHDx40EgygYGB5uTJk07rpy6bNWuWo61+/fomICDAHD582KlvSkqKW+reuXOnkWQeffRRp/Zn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AAAAALfL8yPlAAAAAADkVoRyAAAAAAAsQigHAAAAAMAihHIAAAAAACyS50P51KlTFRERIV9fX9WqVUubN2+2uiQgXVFRUbrrrrsUEBCg4OBgtWvXTnv37nXqc+HCBfXv31+FCxdW/vz59eCDD+rvv/926hMTE6NWrVopX758Cg4O1nPPPafLly/n5EsBMjR+/HjZbDYNHjzY0cZxjZvV0aNH9fDDD6tw4cLy8/NTlSpVtHXrVsdyY4xeeeUVlShRQn5+fmrSpIn279/vtI3Tp0+re/fuCgwMVIECBfTII48oISEhp18K4JCcnKwRI0aoVKlS8vPzU2RkpMaOHasr54bm2EZut3HjRrVu3VohISGy2WxasmSJ03J3HcM///yz6tWrJ19fX4WGhmrixIlZqjdPh/L58+dr6NChGjlypLZv365q1aqpefPmOnnypNWlAWls2LBB/fv3148//qhVq1bp0qVLatasmRITEx19hgwZoqVLl+rzzz/Xhg0bdOzYMXXo0MGxPDk5Wa1atdLFixf1/fffa86cOZo9e7ZeeeUVK14S4GTLli16//33VbVqVad2jmvcjGJjY1WnTh15e3tr+fLl+u233/Tmm2+qYMGCjj4TJ07U22+/renTp+unn36Sv7+/mjdvrgsXLjj6dO/eXb/++qtWrVqlZcuWaePGjerXr58VLwmQJE2YMEHTpk3Tu+++qz179mjChAmaOHGi3nnnHUcfjm3kdomJiapWrZqmTp2a7nJ3HMPx8fFq1qyZwsPDtW3bNr3++usaNWqUZsyY4XrBJg+7++67Tf/+/R3Pk5OTTUhIiImKirKwKiBzTp48aSSZDRs2GGOMOXPmjPH29jaff/65o8+ePXuMJPPDDz8YY4z53//+Zzw8PMyJEyccfaZNm2YCAwNNUlJSzr4A4Apnz541ZcuWNatWrTINGjQwgwYNMsZwXOPm9cILL5i6detmuDwlJcUUL17cvP766462M2fOGLvdbj777DNjjDG//fabkWS2bNni6LN8+XJjs9nM0aNHs6944BpatWpl+vbt69TWoUMH0717d2MMxzZuPpLM4sWLHc/ddQy/9957pmDBgk5/i7zwwgumfPnyLteYZ0fKL168qG3btqlJkyaONg8PDzVp0kQ//PCDhZUBmRMXFydJKlSokCRp27ZtunTpktMxXaFCBYWFhTmO6R9++EFVqlRRsWLFHH2aN2+u+Ph4/frrrzlYPeCsf//+atWqldPxK3Fc4+b11Vdf6c4771SnTp0UHBys6tWr64MPPnAsP3jwoE6cOOF0bAcFBalWrVpOx3aBAgV05513Ovo0adJEHh4e+umnn3LuxQBXqF27ttasWaN9+/ZJknbt2qXvvvtOLVu2lMSxjZufu47hH374QfXr15ePj4+jT/PmzbV3717Fxsa6VJPXjbyg3OzUqVNKTk52+iNOkooVK6bff//doqqAzElJSdHgwYNVp04d3X777ZKkEydOyMfHRwUKFHDqW6xYMZ04ccLRJ71jPnUZYIV58+Zp+/bt2rJlS5plHNe4Wf3555+aNm2ahg4dqhdffFFbtmzRwIED5ePjo169ejmOzfSO3SuP7eDgYKflXl5eKlSoEMc2LDNs2DDFx8erQoUK8vT0VHJyssaNG6fu3btLEsc2bnruOoZPnDihUqVKpdlG6rIrL2e6njwbyoGbWf/+/fXLL7/ou+++s7oU4IYcOXJEgwYN0qpVq+Tr62t1OYDbpKSk6M4779Rrr70mSapevbp++eUXTZ8+Xb169bK4OiDrFixYoOjoaM2dO1eVK1fWzp07NXjwYIWEhHBsA9kkz56+XqRIEXl6eqaZwffvv/9W8eLFLaoKuL4BAwZo2bJlWrdunUqWLOloL168uC5evKgzZ8449b/ymC5evHi6x3zqMiCnbdu2TSdPnlSNGjXk5eUlLy8vbdiwQW+//ba8vLxUrFgxjmvclEqUKKFKlSo5tVWsWFExMTGS/u/YvNbfIcWLF08z+ezly5d1+vRpjm1Y5rnnntOwYcPUtWtXValSRT169NCQIUMUFRUliWMbNz93HcPu/Pskz4ZyHx8f1axZU2vWrHG0paSkaM2aNbr33nstrAxInzFGAwYM0OLFi7V27do0p8PUrFlT3t7eTsf03r17FRMT4zim7733Xu3evdvpQ2TVqlUKDAxM88cjkBMaN26s3bt3a+fOnY7HnXfeqe7duzv+zXGNm1GdOnXS3LZy3759Cg8PlySVKlVKxYsXdzq24+Pj9dNPPzkd22fOnNG2bdscfdauXauUlBTVqlUrB14FkNa5c+fk4eEcETw9PZWSkiKJYxs3P3cdw/fee682btyoS5cuOfqsWrVK5cuXd+nUdUl5e/b1efPmGbvdbmbPnm1+++03069fP1OgQAGnGXyB3OLJJ580QUFBZv369eb48eOOx7lz5xx9nnjiCRMWFmbWrl1rtm7dau69915z7733OpZfvnzZ3H777aZZs2Zm586dZsWKFaZo0aJm+PDhVrwkIF1Xzr5uDMc1bk6bN282Xl5eZty4cWb//v0mOjra5MuXz3z66aeOPuPHjzcFChQwX375pfn5559N27ZtTalSpcz58+cdfVq0aGGqV69ufvrpJ/Pdd9+ZsmXLmm7dulnxkgBjjDG9evUyt912m1m2bJk5ePCgWbRokSlSpIh5/vnnHX04tpHbnT171uzYscPs2LHDSDKTJk0yO3bsMIcPHzbGuOcYPnPmjClWrJjp0aOH+eWXX8y8efNMvnz5zPvvv+9yvXk6lBtjzDvvvGPCwsKMj4+Pufvuu82PP/5odUlAuiSl+5g1a5ajz/nz581TTz1lChYsaPLly2fat29vjh8/7rSdQ4cOmZYtWxo/Pz9TpEgR88wzz5hLly7l8KsBMnZ1KOe4xs1q6dKl5vbbbzd2u91UqFDBzJgxw2l5SkqKGTFihClWrJix2+2mcePGZu/evU59/v33X9OtWzeTP39+ExgYaPr06WPOnj2bky8DcBIfH28GDRpkwsLCjK+vryldurR56aWXnG77xLGN3G7dunXp/l3dq1cvY4z7juFdu3aZunXrGrvdbm677TYzfvz4LNVrM8YYF0f8AQAAAACAG+TZa8oBAAAAAMjtCOUAAAAAAFiEUA4AAAAAgEUI5QAAAAAAWIRQDgAAAACARQjlAAAAAABYhFAOAAAAAIBFCOUAAAAAAFiEUA4AAK5p/fr1stlsOnPmTLbuZ+/evSpevLjOnj2brftxh65du+rNN9+0ugwAQB5AKAcA3NJ++OEHeXp6qlWrVlaXkiNsNpvjERQUpDp16mjt2rXXXKd27do6fvy4goKCsrW24cOH6+mnn1ZAQICk//syIL3HiRMnsrWW63n55Zc1btw4xcXFWVoHAODmRygHANzSZs6cqaefflobN27UsWPHsnVfxhhdvnw5W/eRGbNmzdLx48e1adMmFSlSRA888ID+/PPPdPteunRJPj4+Kl68uGw2W7bVFBMTo2XLlql3795plu3du1fHjx93egQHB2d5XxcvXryBSv9z++23KzIyUp9++ukNbwsAcGsjlAMAblkJCQmaP3++nnzySbVq1UqzZ892LHvooYfUpUsXp/6XLl1SkSJF9PHHH0uSUlJSFBUVpVKlSsnPz0/VqlXTF1984eifOtK7fPly1axZU3a7Xd99953++OMPtW3bVsWKFVP+/Pl11113afXq1U77On78uFq1aiU/Pz+VKlVKc+fOVUREhKZMmeLoc+bMGT366KMqWrSoAgMDdd9992nXrl3Xfd0FChRQ8eLFdfvtt2vatGk6f/68Vq1aJem/kfRp06apTZs28vf317hx49I9fX3Tpk1q2LCh8uXLp4IFC6p58+aKjY3N1PuSngULFqhatWq67bbb0iwLDg5W8eLFnR4eHv/9CdOwYUMNHjzYqX+7du2cwn1ERITGjh2rnj17KjAwUP369ZMkLVy4UJUrV5bdbldERESa09Hfe+89lS1bVr6+vipWrJg6duzotLx169aaN2/eNV8XAADXQygHANyyFixYoAoVKqh8+fJ6+OGH9dFHH8kYI0nq3r27li5dqoSEBEf/lStX6ty5c2rfvr0kKSoqSh9//LGmT5+uX3/9VUOGDNHDDz+sDRs2OO1n2LBhGj9+vPbs2aOqVasqISFB999/v9asWaMdO3aoRYsWat26tWJiYhzr9OzZU8eOHdP69eu1cOFCzZgxQydPnnTabqdOnXTy5EktX75c27ZtU40aNdS4cWOdPn060++Bn5+fJOfR41GjRql9+/bavXu3+vbtm2adnTt3qnHjxqpUqZJ++OEHfffdd2rdurWSk5Ndel+u9O233+rOO+/MdN2ueuONN1StWjXt2LFDI0aM0LZt29S5c2d17dpVu3fv1qhRozRixAjHFzNbt27VwIEDNWbMGO3du1crVqxQ/fr1nbZ59913a/PmzUpKSsq2ugEAtwADAMAtqnbt2mbKlCnGGGMuXbpkihQpYtatW+f0/OOPP3b079atm+nSpYsxxpgLFy6YfPnyme+//95pm4888ojp1q2bMcaYdevWGUlmyZIl162lcuXK5p133jHGGLNnzx4jyWzZssWxfP/+/UaSmTx5sjHGmG+//dYEBgaaCxcuOG0nMjLSvP/++xnuR5JZvHixMcaYxMRE89RTTxlPT0+za9cux/LBgwc7rZP6OmJjYx3vQ506ddLdfmbel/RUq1bNjBkzJt39+vv7Oz0qVark6NOgQQMzaNAgp/Xatm1revXq5XgeHh5u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\n",
      "text/plain": [
       "<Figure size 1200x800 with 1 Axes>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "plt.figure(figsize = (12,8))\n",
    "sns.barplot(data = paris_Elysee_accommodates,\n",
    "           x = \"price\",\n",
    "           y = paris_Elysee_accommodates.index,\n",
    "           color = \"green\",\n",
    "           orient = \"h\",\n",
    "           order = paris_Elysee_accommodates.index)\n",
    "plt.title(\"Average Price of AirBnB in Elysee -- vs -- Number of People that can stay\")\n",
    "plt.xlabel(\"Average Price (Euros)\")\n",
    "plt.ylabel(\"Number of People that can stay\")\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 47,
   "id": "fb4706cc",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2025-02-10T03:28:20.680889Z",
     "iopub.status.busy": "2025-02-10T03:28:20.680653Z",
     "iopub.status.idle": "2025-02-10T03:28:20.683321Z",
     "shell.execute_reply": "2025-02-10T03:28:20.682734Z"
    },
    "papermill": {
     "duration": 0.017921,
     "end_time": "2025-02-10T03:28:20.684564",
     "exception": false,
     "start_time": "2025-02-10T03:28:20.666643",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [],
   "source": [
    "# The AirBnBs with 14 people accomodation is the costliest, even costlier than 16 accomodates. \n",
    "# the same trend is spotted for 11 and 12 accomodates. Every other accomodation looks reasonable."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 48,
   "id": "e95d7384",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2025-02-10T03:28:20.712124Z",
     "iopub.status.busy": "2025-02-10T03:28:20.711915Z",
     "iopub.status.idle": "2025-02-10T03:28:20.718986Z",
     "shell.execute_reply": "2025-02-10T03:28:20.718215Z"
    },
    "papermill": {
     "duration": 0.022228,
     "end_time": "2025-02-10T03:28:20.720145",
     "exception": false,
     "start_time": "2025-02-10T03:28:20.697917",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>neighbourhood</th>\n",
       "      <th>price</th>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>host_since</th>\n",
       "      <th></th>\n",
       "      <th></th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>2008-12-31</th>\n",
       "      <td>4</td>\n",
       "      <td>77.750000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2009-12-31</th>\n",
       "      <td>106</td>\n",
       "      <td>159.641509</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2010-12-31</th>\n",
       "      <td>416</td>\n",
       "      <td>125.031250</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2011-12-31</th>\n",
       "      <td>1339</td>\n",
       "      <td>124.828230</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2012-12-31</th>\n",
       "      <td>4592</td>\n",
       "      <td>111.578615</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2013-12-31</th>\n",
       "      <td>8142</td>\n",
       "      <td>107.096414</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2014-12-31</th>\n",
       "      <td>10922</td>\n",
       "      <td>100.253800</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2015-12-31</th>\n",
       "      <td>12147</td>\n",
       "      <td>103.646250</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2016-12-31</th>\n",
       "      <td>8867</td>\n",
       "      <td>114.211345</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2017-12-31</th>\n",
       "      <td>4585</td>\n",
       "      <td>108.658888</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2018-12-31</th>\n",
       "      <td>4294</td>\n",
       "      <td>138.209362</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2019-12-31</th>\n",
       "      <td>5685</td>\n",
       "      <td>129.962533</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2020-12-31</th>\n",
       "      <td>3363</td>\n",
       "      <td>143.517098</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2021-12-31</th>\n",
       "      <td>133</td>\n",
       "      <td>93.488722</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "            neighbourhood       price\n",
       "host_since                           \n",
       "2008-12-31              4   77.750000\n",
       "2009-12-31            106  159.641509\n",
       "2010-12-31            416  125.031250\n",
       "2011-12-31           1339  124.828230\n",
       "2012-12-31           4592  111.578615\n",
       "2013-12-31           8142  107.096414\n",
       "2014-12-31          10922  100.253800\n",
       "2015-12-31          12147  103.646250\n",
       "2016-12-31           8867  114.211345\n",
       "2017-12-31           4585  108.658888\n",
       "2018-12-31           4294  138.209362\n",
       "2019-12-31           5685  129.962533\n",
       "2020-12-31           3363  143.517098\n",
       "2021-12-31            133   93.488722"
      ]
     },
     "execution_count": 48,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "paris_listings_over_time"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 49,
   "id": "540b96d4",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2025-02-10T03:28:20.785386Z",
     "iopub.status.busy": "2025-02-10T03:28:20.785098Z",
     "iopub.status.idle": "2025-02-10T03:28:21.031195Z",
     "shell.execute_reply": "2025-02-10T03:28:21.030414Z"
    },
    "papermill": {
     "duration": 0.298656,
     "end_time": "2025-02-10T03:28:21.032467",
     "exception": false,
     "start_time": "2025-02-10T03:28:20.733811",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [
    {
     "data": {
      "image/png": "iVBORw0KGgoAAAANSUhEUgAAA/8AAAK9CAYAAABy5dyFAAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjcuNSwgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy/xnp5ZAAAACXBIWXMAAA9hAAAPYQGoP6dpAACnq0lEQVR4nOzdd3xb5dn/8a/kvffMJgnZC2gDZQWygDBCKRAIBUIK7a+sAKVAnxIgtOxVxkPgKZC0TygFSnmAlhAzUyBlBFIICRBCyPKe8rZsnd8ftyXbZFmJ5CPJn/frlZePjo7kS/KJrevc931dDsuyLAEAAAAAgIjltDsAAAAAAAAQXCT/AAAAAABEOJJ/AAAAAAAiHMk/AAAAAAARjuQfAAAAAIAIR/IPAAAAAECEI/kHAAAAACDCkfwDAAAAABDhSP4BAAAAAIhwJP8AAPTCtGnTNG3atIA+53fffSeHw6Fly5YF9HkP1N13362DDjpIUVFRmjx5ckCe8+2335bD4dDbb78dkOdDcF144YUaOnSo3WEAAAKI5B8AYLtly5bJ4XD4/sXHx+vggw/WZZddprKyMrvD61P//Oc/dfPNN9v2/VetWqVf//rXOvLII/XUU0/ptttu69XjzjrrLDkcDl133XUBiWPatGk9zonY2FgNGzZMl1xyibZv377fzzt06NBdzrWRI0fq2muvVXV1dUBiDxfFxcW6+eabtW7dOrtDAQD0AYdlWZbdQQAA+rdly5ZpwYIFWrJkiYYNG6aWlha9++67+vOf/6whQ4Zo/fr1SkxMtDVG76h/IEeuLctSa2urYmJiFBUVJUm67LLL9Mgjj8iuP8/XX3+97r77bjU3Nys2NrZXj3G5XMrLy1N+fr46Ojq0detWORyOHsd4PB61tbUpNjZWTue+xx6mTZumzZs36/bbb5cktbW1acOGDVq6dKmysrK0cePG/Tonhg4dqoyMDF1zzTWSpJaWFq1du1Z//OMfNWXKFH344Yd+P2e4+vjjj/WDH/xATz31lC688MIe97ndbnk8HsXFxdkTHAAg4KLtDgAAAK8TTzxRhx12mCTpZz/7mbKysnTffffp//7v/3TOOefYHF3gtLe3y+PxKDY2VvHx8XaH00N5ebkSEhJ6nfhL0t/+9jd1dHToySef1PHHH6/Vq1fr2GOP7XGM0+ns1WttamryJfVpaWk677zzetw/bNgwXXbZZXrvvfc0c+bMXsfY3YABA3o8789+9jMlJyfrnnvu0aZNmzRy5Mj9et5Q1NjYqKSkJL8fFxMTE4RoAAB2Yto/ACBkHX/88ZKkLVu2SDJJ86233qrhw4crLi5OQ4cO1W9+8xu1trb2eNzQoUN18skna9WqVZo8ebLi4+M1duxYvfDCCz2Ou/nmm3cZoZa6liF89913e4ytra1Nixcv1qGHHqq0tDQlJSXp6KOP1ltvvdXjOO+6/nvuuUcPPPCAL/YNGzbssub/wgsv1COPPCJJPaamW5aloUOH6rTTTtsljpaWFqWlpennP//5Xt/L3rx3DodDTz31lBobG33fuzf1CFasWKGZM2fquOOO05gxY7RixYpdjtndmv9p06Zp/PjxWrt2rY455hglJibqN7/5zV6/V35+viQpOrpr/ML7c/zmm2904YUXKj09XWlpaVqwYIGampr2Gf+enre0tFQLFizQwIEDFRcXp4KCAp122ml7PS+83nzzTR199NFKSkpSenq6TjvtNG3cuNF3//PPPy+Hw6F33nlnl8c+9thjcjgcWr9+vW/fl19+qZ/85CfKzMxUfHy8DjvsML300ks9Huc9b9955x398pe/VG5urgYOHLjb+N5++2394Ac/kCQtWLBgl5/399f8dz+PH3nkER100EFKTEzUrFmztH37dlmWpVtvvVUDBw5UQkKCTjvttN0uo3j11Vd970tKSormzJmjL774Yp/vJwDgwDHyDwAIWZs3b5YkZWVlSTIjtMuXL9dPfvITXXPNNfrggw90++23a+PGjfr73//e47GbNm3S2WefrV/84he64IIL9NRTT+nMM8/UypUr93vEuDuXy6U//vGPOuecc3TxxRervr5eTzzxhGbPnq0PP/xwl0J5Tz31lFpaWnTJJZcoLi5OmZmZ8ng8PY75+c9/ruLiYhUVFenPf/6zb7/D4dB5552nu+66S9XV1crMzPTd9/LLL8vlcu0yQv59vXnv/vznP+vxxx/Xhx9+qD/+8Y+SpB/96Ed7fd7i4mK99dZbWr58uSTpnHPO0f3336+HH364V7MHqqqqdOKJJ2revHk677zzlJeX57uvo6NDlZWVksw09I0bN+qmm27SiBEjdOSRR+7yXGeddZaGDRum22+/XZ988on++Mc/Kjc3V3feeWeP49xut+95W1pa9Omnn+q+++7TMccco2HDhvmOO+OMM/TFF1/o8ssv19ChQ1VeXq6ioiJt27Ztr8XwXn/9dZ144ok66KCDdPPNN6u5uVkPPfSQjjzySH3yyScaOnSo5syZo+TkZD377LO7zJL461//qnHjxmn8+PGSpC+++EJHHnmkBgwYoOuvv15JSUl69tlnNXfuXP3tb3/T6aef3uPxv/zlL5WTk6PFixersbFxtzGOGTNGS5Ys0eLFi3XJJZfo6KOPlrTvn/eKFSvU1tamyy+/XNXV1brrrrt01lln6fjjj9fbb7+t6667Tt98840eeugh/epXv9KTTz7pe+yf//xnXXDBBZo9e7buvPNONTU16dFHH9VRRx2lTz/9lAKDABBsFgAANnvqqacsSdbrr79uVVRUWNu3b7eeeeYZKysry0pISLB27NhhrVu3zpJk/exnP+vx2F/96leWJOvNN9/07RsyZIglyfrb3/7m21dXV2cVFBRYU6ZM8e276aabrN39KfTGs2XLFt++Y4891jr22GN9t9vb263W1tYej6upqbHy8vKsiy66yLdvy5YtliQrNTXVKi8v73G8976nnnrKt+/SSy/dbUxfffWVJcl69NFHe+w/9dRTraFDh1oej2eXx3j5895dcMEFVlJS0h6f6/vuueceKyEhwXK5XJZlWdbXX39tSbL+/ve/9zjurbfesiRZb731lm/fsccea0myli5dusvzeu/7/r8xY8ZY3377bY9jvT/H7u+7ZVnW6aefbmVlZfXY5z03vv/vyCOPtCorK33H1dTUWJKsu+++u9fvhdfkyZOt3Nxcq6qqyrfvP//5j+V0Oq3zzz/ft++cc86xcnNzrfb2dt++kpISy+l0WkuWLPHtmz59ujVhwgSrpaXFt8/j8Vg/+tGPrJEjR/r2ec/bo446qsdz7slHH320y/nndcEFF1hDhgzx3faeqzk5OVZtba1v/w033GBJsiZNmmS53e4ery02NtYXc319vZWenm5dfPHFPb5PaWmplZaWtst+AEDgMe0fABAyZsyYoZycHA0aNEjz5s1TcnKy/v73v2vAgAH65z//KUm6+uqrezzGW7jtH//4R4/9hYWFPUZEU1NTdf755+vTTz9VaWnpAccaFRXlG9n2eDyqrq5We3u7DjvsMH3yySe7HH/GGWcoJydnv7/fwQcfrKlTp/aYUl9dXa1XX31V8+fP3+3yBS9/3zt/rFixQnPmzFFKSookaeTIkTr00EN3O/V/d+Li4rRgwYLd3jd06FAVFRWpqKhIr776qh544AHV1dXpxBNPVEVFxS7H/+IXv+hx++ijj1ZVVZVcLleP/VOnTvU97yuvvKLf//73+uKLL3TqqaequblZknx1D95++23V1NT06rVIUklJidatW6cLL7ywxwyNiRMnaubMmb6fhSSdffbZKi8v77EU4vnnn5fH49HZZ58tyfyM33zzTZ111lmqr69XZWWlKisrVVVVpdmzZ2vTpk3auXNnjxguvvhiXwHJQDvzzDOVlpbmuz116lRJ0nnnnddjycTUqVPV1tbmi62oqEi1tbU655xzfK+hsrJSUVFRmjp16i7LZQAAgce0fwBAyHjkkUd08MEHKzo6Wnl5eRo1apSvMvzWrVvldDo1YsSIHo/Jz89Xenq6tm7d2mP/iBEjdkmIDz74YElm/bJ3jfeBWL58ue699159+eWXcrvdvv3dp47vbZ+/zj//fF122WXaunWrhgwZoueee05ut1s//elP9/o4f9+73tq4caM+/fRTnX/++frmm298+6dNm6ZHHnlELpdLqampe32OAQMG7HF5QFJSkmbMmOG7fcIJJ+ioo47SYYcdpjvuuEP33ntvj+MHDx7c43ZGRoYkqaampkcc2dnZPZ53zpw5GjVqlH7yk5/oj3/8oy6//HLFxcXpzjvv1DXXXKO8vDwdfvjhOvnkk3X++efv9dzxvpejRo3a5b4xY8botdde8xXhO+GEE5SWlqa//vWvmj59uiQz5X/y5Mm+c/Wbb76RZVm68cYbdeONN+72e5aXl2vAgAG+24E41/bk+++x90LAoEGDdrvfe+Fk06ZNkrrqeHzfvs4TAMCBI/kHAISMH/7wh75q/3uytxFuf+3puTo6Ovb52P/93//VhRdeqLlz5+raa69Vbm6uoqKidPvtt/tqFXSXkJBwwPHOmzdPV111lVasWKHf/OY3+t///V8ddthhu000dyeQ751k3gNJuuqqq3TVVVftcv/f/va3PY7qe/n7vngLLK5evXqX+/Y02m31om2iN/levXq1Lr/8cknSokWLdMopp+jFF1/Ua6+9phtvvFG333673nzzTU2ZMsWvuHcnLi5Oc+fO1d///nf993//t8rKyvTee+/ptttu8x3jrQvxq1/9SrNnz97t83z/ok4gzrU92dN7vK/33vs6/vznP+/24kn3WQMAgODgNy0AICwMGTJEHo9HmzZt0pgxY3z7y8rKVFtbqyFDhvQ43jti2j3h/frrryXJV1jMOzJcW1ur9PR033G9GQl//vnnddBBB+mFF17o8T1uuukmv19bd3tL0DMzMzVnzhytWLFC8+fP13vvvacHHnhgn8/p73vXG5Zl6emnn9Zxxx2nX/7yl7vcf+utt2rFihX7TP73R0dHhxoaGgL6nO3t7ZK0y/MOHz5c11xzja655hpt2rRJkydP1r333uu78PF93vfyq6++2uW+L7/8UtnZ2T1a75199tlavny53njjDW3cuFGWZfmm/EvSQQcdJMm03us+WyEQAn0xaG+GDx8uScrNzQ346wAA9A5r/gEAYeGkk06SpF2S3fvuu0+SmbrdXXFxcY8OAC6XS3/60580efJk38ijNyHpPorc2Njoq1y/N96Rzu6jyh988IHWrFnT25e0W97EsLa2drf3//SnP9WGDRt07bXXKioqSvPmzdvnc/r73vXGe++9p++++04LFizQT37yk13+nX322XrrrbdUXFzs93PvzVtvvaWGhgZNmjQpoM/78ssvS5LveZuamtTS0tLjmOHDhyslJWWX1pLdFRQUaPLkyVq+fHmPn+H69eu1atUq38/Ca8aMGcrMzNRf//pX/fWvf9UPf/jDHtP2c3NzNW3aND322GMqKSnZ5fvtrvZBb+3rXAuk2bNnKzU1VbfddluPJTJeB/I6AAC9w8g/ACAsTJo0SRdccIEef/xx1dbW6thjj9WHH36o5cuXa+7cuTruuON6HH/wwQdr4cKF+uijj5SXl6cnn3xSZWVleuqpp3zHzJo1S4MHD9bChQt9yfSTTz6pnJwcbdu2ba/xnHzyyXrhhRd0+umna86cOdqyZYuWLl2qsWPHHtCo9KGHHipJuuKKKzR79uxdEvw5c+YoKytLzz33nE488UTl5ubu8zn9fe96Y8WKFYqKitrjhYNTTz1V//Vf/6Vnnnlml0KDvVVXV+cbYW9vb9dXX32lRx99VAkJCbr++uv36zklaefOnb7nbWtr03/+8x899thjys7O9k35//rrrzV9+nSdddZZGjt2rKKjo/X3v/9dZWVl+7zgcvfdd+vEE0/UEUccoYULF/pa/aWlpenmm2/ucWxMTIx+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96WZ+p+fJaWl2B0VQth+Jf+vvfaaRo0aJUm7FPwDANhsZ5n5mp8tRUfZGwsAAAietGTz97600hT/O3QsM/6wR35P+7/33nv15JNPauPGjXr77bf11ltv+f69+eabwYjR54477pDD4dCiRYt8+1paWnTppZcqKytLycnJOuOMM1RWVtbjcdu2bdOcOXOUmJio3NxcXXvttWpvb+9xzNtvv61DDjlEcXFxGjFihJYtWxbU1wIAQdHUIlW7zDbt/QAAiHwHDTAX+xubu5b9Abvhd/IfFxenI488Mhix7NVHH32kxx57TBMnTuyx/6qrrtLLL7+s5557Tu+8846Ki4v14x//2Hd/R0eH5syZo7a2Nr3//vtavny5li1bpsWLF/uO2bJli+bMmaPjjjtO69at06JFi/Szn/1Mr732Wp+9PgAICO+of1aalBBvbywAACD4YmKkgwaa7e92Sq1t9saDkOX3tP8rr7xSDz30kB588MFgxLNbDQ0Nmj9/vv7nf/5Hv/vd73z76+rq9MQTT+jpp5/W8ccfL0l66qmnNGbMGP373//W4YcfrlWrVmnDhg16/fXXlZeXp8mTJ+vWW2/Vddddp5tvvlmxsbFaunSphg0bpnvvvVeSNGbMGL377ru6//77NXv27N3G1NraqtbWVt9tl8uMtLndbrndVNsMdd6fET+r/i3izoP2DkWXVskhqT0vS1akvK4gi7jzAPuF8wAS5wGMsDwPstIUlZwoZ0OTPJu2qWPUELsjCnvhch74E5/DsizLnyc//fTT9eabbyorK0vjxo3bpeDfCy+84M/T9coFF1ygzMxM3X///Zo2bZomT56sBx54QG+++aamT5+umpoapaen+44fMmSIFi1apKuuukqLFy/WSy+9pHXr1vnu37Jliw466CB98sknmjJlio455hgdcsgheuCBB3zHPPXUU1q0aJHq6up2G9PNN9+sW265ZZf9Tz/9tBITEwP10gGg14bHJWt8QppcHW69Vc+0PwAA+pO0qBgdm5wjh8Oh9xsqVdHeuu8HIew1NTXp3HPPVV1dnVJTU/d6rN8j/+np6T2m1QfbM888o08++UQfffTRLveVlpYqNja2R+IvSXl5eSotLfUdk5eXt8v93vv2dozL5VJzc7MSEhJ2+d433HCDrr76at9tl8ulQYMGadasWft802E/t9utoqIizZw5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a26++WZNmDBBXq9Xp5xySr/v9emnn+rcc89VYmKiioqKdM8994SrWThKkcqDd999VxdffLGGDx+ulJQUnXLKKXr++efD2TQchUh+HnT79ttvlZaWpoyMjBC3BgMVyTywLEv33XefxowZI6/Xq4KCAv3hD38IV9NwFCKZB8uXL9dZZ52ltLQ05eTkaPr06dq8eXOYWoajEYo8+OSTTzRjxgwVFRUpKSlJ48aN00MPPdTnvd59912ddtpp8nq9OvbYY/XMM8+Eu3k4CpHKhddee02TJ09WTk6OfD6fSktLtXz58oi08WhQ/CMmrV69WnPnztV///tfrVixQn6/X1OmTNHevXuD19x666168803tXTpUq1evVpVVVW67LLL+tzr5z//ua644op+36epqUlTpkxRcXGxysvLde+99+rOO+/U448/Hra24chFKg8++OADnXTSSXr11Vf16aefavbs2br66qu1bNmysLUNRy5SedDN7/drxowZOvfcc0PeFgxcJPPglltu0ZNPPqn77rtPX3/9td544w2dccYZYWkXjk6k8qCyslIXX3yxfvCDH2j9+vVavny56uvr+70PIi8UeVBeXq7c3Fw999xz+uKLL/S73/1OixYt0l//+tfgNZWVlbrwwgs1adIkrV+/XvPnz9ecOXOisugbqiKVC++9954mT56st99+W+Xl5Zo0aZKmTZumdevWRbS938kC4kBtba0lyVq9erVlWZbV0NBgeTwea+nSpcFrvvrqK0uStWbNmj7//OLFi62TTz65z/lHHnnEyszMtNrb24PnbrvtNmvs2LGhbwQGLVx50J+f/OQn1uzZs0MSN0Ir3HmwcOFC66qrrrKefvppKz09PdThI0TClQdffvml5Xa7ra+//jpssSN0wpUHS5cutdxut9XV1RU898Ybb1gOh8Pq6OgIfUMwKIPNg2433XSTNWnSpODxwoULrfHjx/e65oorrrCmTp0a4hYgVMKVC/054YQTrLvuuis0gYcII/+IC42NjZKkYcOGSTLf0Pn9fl1wwQXBa44//niNHDlSa9asOeL7rlmzRhMnTlRCQkLw3NSpU1VRUaE9e/aEKHqESrjy4FDv1f0+iC7hzIOysjItXbpUS5YsCV3ACItw5cGbb76p0aNHa9myZSopKdGoUaM0Z84c7d69O7QNQEiEKw8mTJggp9Opp59+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\n",
      "text/plain": [
       "<Figure size 1200x800 with 1 Axes>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "plt.figure(figsize = (12,8))\n",
    "sns.lineplot(data = paris_listings_over_time[\"neighbourhood\"],color = \"pink\")\n",
    "plt.xlabel(\"Years\")\n",
    "plt.ylabel(\"Number of AirBnBs and Localities\")\n",
    "plt.title(\"Popularity of AirBnBs over time\")\n",
    "plt.grid(True)\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 50,
   "id": "936017b0",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2025-02-10T03:28:21.063459Z",
     "iopub.status.busy": "2025-02-10T03:28:21.063210Z",
     "iopub.status.idle": "2025-02-10T03:28:21.289913Z",
     "shell.execute_reply": "2025-02-10T03:28:21.289092Z"
    },
    "papermill": {
     "duration": 0.243576,
     "end_time": "2025-02-10T03:28:21.291160",
     "exception": false,
     "start_time": "2025-02-10T03:28:21.047584",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [
    {
     "data": {
      "image/png": "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VaVKFW3dulXTpk3TM88845G4b7rpJn344Ye6+uqrdc8992jJkiV67rnntGHDhoz9+B9//LEmTpyo3377Te+++64k6fLLLz/v4+7Zs0dz587Vhx9+KEm6/vrr9corr+iNN97I1Wr9oUOH1L17dw0YMECDBg1S2bJlM25LS0vT33//Lcn8jDds2JDxJlCrVq3Oeqxrr71WVatW1XPPPafly5fr3XffVZkyZTRmzJhs90tJScl43DNnzmjFihV6+eWX1bZtW1WtWjXjfv3799e6det0++23q0qVKjpw4IASEhK0c+fO8zZpmzVrlrp3765q1arp8ccf1+nTp/X666+rVatWWr58uapUqaKePXuqaNGimjx58llVCV988YXq1aun+vXrS5LWrVunVq1aqUKFCnrggQdUpEgRTZ48WX379tWUKVPUr1+/bF9/6623qnTp0nr00Ud18uTJHGOsU6eOnnzyST366KMaOXKk2rRpI+nCv+/4+HglJyfr9ttv1+HDh/XCCy/o2muvVceOHTVv3jzdf//92rJli15//XWNHj0625tKH3/8sYYOHapu3bppzJgxOnXqlN566y21bt1aK1asoPEdAFyIBQAIekuXLrUkWQkJCZZlWVZ6erpVsWJF684778y4z4wZMyxJ1rRp07J9bY8ePaxq1aplfP7xxx9bISEh1s8//5ztfhMmTLAkWb/88kvGdZKskJAQa926dWfFdOrUqWyfJycnW/Xr17c6duyYcd2yZcssSdb//d//ZbvvsGHDLEnWY489lnHdjTfeaJUrV876+++/s913wIABVkxMzFnP92+VK1e2unbtah08eNA6ePCgtWrVKmvAgAGWJOv222+3LMuytm3bZkmyoqOjrQMHDmT7evdtkyZNyriubdu2VrFixawdO3Zku296erpH4l65cqUlybrpppuyXT969GhLkjVnzpyM64YOHWoVKVLkvD+DrMaOHWtFRUVZiYmJlmVZ1qZNmyxJ1jfffJPtfnPnzrUkWXPnzs24rl27dpYka8KECWc9rvu2f3/UqVPH+vPPP7Pd97HHHrMkWcOHD892fb9+/aySJUtmu65y5co5Pm6rVq2y/WyPHDliSbJefPHFXP8s3Bo1amSVKVPGOnToUMZ1q1atskJCQqwhQ4ZkXHf99ddbZcqUsVJTUzOu27t3rxUSEmI9+eSTGdd1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nNM+aW8rXbq02rZtq/fff187d+7Mdpt7lbKgcffo0UOSNG7cuGzXu6sgcuqyfyG//PKLtm/frv/85z85njPXXXed5s6dqz179uT5sc9n7ty5OnHiRLaqDk+YNm2aJGU87qlTp3TmzJls96levbqKFSt21sjCrMqVK6dGjRrpww8/1NGjRzOuX7t2rWbOnJnxu3Dr3LmzYmNj9cUXX+iLL77QZZddlq28vUyZMmrfvr3efvtt7d2796znK8g5637zIGuc3tKtWzdFR0fr2WefVUpKylm32/FvDwD8DSvtABDEvvvuOx0/fly9e/fO8fYWLVqodOnSio+Pz0jOr7vuOr3++ut67LHHdMkll2RbwZWkwYMHa/Lkybrllls0d+5ctWrVSmlpafrjjz80efJkzZgxQ5deeul54+rZs6defvllXXHFFbrhhht04MABjR8/XjVq1NDq1asz7te0aVP1799f48aN06FDhzJGvm3atElS9hXF559/XnPnzlXz5s01YsQI1a1bV4cPH9by5cs1a9asHGdLe9trr72m1q1bq0mTJho5cqSqVq2q7du364cfftDKlSsLHHfDhg01dOhQTZw4UUePHlW7du3022+/6cMPP1Tfvn0zGg/mRXx8vEJDQ8+Z8Pfu3VsPP/ywPv/887Ma4OXWsWPHMla0U1NTtXHjRr311luKiorSAw88kK/HlMy8e/fjJicna9WqVXr77bdVqlSpjNL4TZs2qVOnTrr22mtVt25dhYWF6ZtvvtH+/fszxhqey4svvqju3burZcuWuvHGGzNGvsXExOjxxx/Pdt/w8HBdddVV+vzzz3Xy5EmNHTv2rMcbP368WrdurUsuuUQjRoxQtWrVtH//fi1evFi7du3SqlWr8vVzqF69uooXL64JEyaoWLFiKlKkiJo3b+6VPfHR0dF66623NHjwYDVp0kQDBgxQ6dKltXPnTv3www9q1aqV3njjDY8/LwAEFLva1gMA7NerVy+rUKFC1smTJ895n2HDhlnh4eEZY7HS09OtSpUqWZKsp59+OsevSU5OtsaMGWPVq1fPioyMtEqUKGE1bdrUeuKJJ6xjx45l3E+Sddttt+X4GO+9955Vs2ZNKzIy0qpdu7Y1adKkjBFfWZ08edK67bbbrNjYWKto0aJW3759rY0bN1qSrOeffz7bfffv32/ddtttVqVKlazw8HArLi7O6tSpkzVx4sQL/qwqV65s9ezZ87z3cY/HymlcWE4j3yzLstauXWv169fPKl68uFWoUCGrVq1a1iOPPOKxuFNSUqwnnnjCqlq1qhUeHm5VqlTJevDBB7ONEbOs3I18S05OtkqWLGm1adPmvPerWrWq1bhxY8uyzj3yrV69ejl+7b9HvrlcLis2Ntbq3bu3tWzZsmz3dZ8PBw8ezHa9ewTatm3bMq7798i3kJAQq0yZMtb1119vbdmyJeN+f//9t3XbbbdZtWvXtooUKWLFxMRYzZs3tyZPnnze79lt1qxZVqtWrayoqCgrOjra6tWrl7V+/foc75uQkJDxPf7111853mfr1q3WkCFDrLi4OCs8PNyqUKGCdeWVV1pfffXVWd/v77//nqsYLcuyvv32W6tu3bpWWFhYtvPyXCPf/n1Ou3+vX375ZbbrzxXL3LlzrW7dulkxMTFWoUKFrOrVq1vDhg2zli5dmuuYASBYuSwrF11aAADwIytXrlTjxo31ySefaODAgXaHAwAAkG/saQcA+DX3fO2sxo0bp5CQELVt29aGiAAAADyHPe0AAL/2wgsvaNmyZerQoYPCwsL0008/6aefftLIkSPPGi8HAADgbyiPBwD4tYSEBD3xxBNav369Tpw4oYsuukiDBw/Www8/zAxoAADg90jaAQAAAABwKPa0AwAAAADgUCTtAAAAAAA4FJv9JKWnp2vPnj0qVqyYXC6X3eEAAAAAAAKcZVk6fvy4ypcvr5CQc6+nk7RL2rNnDx2GAQAAAAA+99dff6lixYrnvJ2kXVKxYsUkmR9WdHS0zdHgfFJSUjRz5kx17dpV4eHhdocDm3AeQOI8gMF5ADfOBUicBzD85TxITExUpUqVMvLRcyFplzJK4qOjo0naHS4lJUWFCxdWdHS0o/8Bwrs4DyBxHsDgPIAb5wIkzgMY/nYeXGiLNo3oAAAAAABwKJJ2AAAAAAAciqQdAAAAAACHImkHAAAAAMChSNoBAAAAAHAoknYAAAAAAByKpB0AAAAAAIciaQcAAAAAwKFI2gEAAAAAcCiSdgAAAAAAHIqkHQAAAAAAhyJpBwAAAADAoUjaAQAAAABwKJJ2AAAAAAAciqQdAAAAAACHImkHAAAAAMChSNoBAAAAAHAoknYAAAAAABzK1qR9wYIF6tWrl8qXLy+Xy6WpU6eedZ8NGzaod+/eiomJUZEiRdSsWTPt3Lkz4/YzZ87otttuU8mSJVW0aFH1799f+/fv9+F3AQAAAACAd9iatJ88eVINGzbU+PHjc7x969atat26tWrXrq158+Zp9erVeuSRR1SoUKGM+9x1112aNm2avvzyS82fP1979uzRVVdd5atvAQAAAAAArwmz88m7d++u7t27n/P2hx9+WD169NALL7yQcV316tUzjo8dO6b33ntPn376qTp27ChJmjRpkurUqaNff/1VLVq08F7wAAAAAAB4ma1J+/mkp6frhx9+0H333adu3bppxYoVqlq1qh588EH17dtXkrRs2TKlpKSoc+fOGV9Xu3ZtXXTRRVq8ePE5k/akpCQlJSVlfJ6YmChJSklJUUpKive+KRSY+/fD7ym4cR5A4jyAwXkAN84FSJwHMPzlPMhtfI5N2g8cOKATJ07o+eef19NPP60xY8Zo+vTpuuqqqzR37ly1a9dO+/btU0REhIoXL57ta8uWLat9+/ad87Gfe+45PfHEE2ddP3PmTBUuXNjT3wq8ICEhwe4Q4ACcB5A4D2BwHsCNcwES5wEMp58Hp06dytX9HJu0p6enS5L69Omju+66S5LUqFEjLVq0SBMmTFC7du3y/dgPPvig7r777ozPExMTValSJXXt2lXR0dEFCxxelbpjh36eM0etBw1SeHi43eHAJikpKUpISFCXLl04D4IY5wEkzgNk4lyAxHkAw1/OA3fF94U4NmkvVaqUwsLCVLdu3WzX16lTRwsXLpQkxcXFKTk5WUePHs222r5//37FxcWd87EjIyMVGRl51vXh4eGO/qUGvfR0hXXqpPaHDsnVu7fCz/M7RnDg3ywkzgMYnAdw41yAxHkAw+nnQW5jc+yc9oiICDVr1kwbN27Mdv2mTZtUuXJlSVLTpk0VHh6u2bNnZ9y+ceNG7dy5Uy1btvRpvPCB/fvl2rlTESdPypXldw4AAAAAgcrWlfYTJ05oy5YtGZ9v27ZNK1euVGxsrC666CLde++9uu6669S2bVt16NBB06dP17Rp0zRv3jxJUkxMjG688Ubdfffdio2NVXR0tG6//Xa1bNmSzvGBaOfOjMOQWbOkgQNtDAYAAAAAvM/WpH3p0qXq0KFDxufufeZDhw7VBx98oH79+mnChAl67rnndMcdd6hWrVqaMmWKWrdunfE1r7zyikJC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\n",
      "text/plain": [
       "<Figure size 1200x800 with 1 Axes>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "plt.figure(figsize = (12,8))\n",
    "sns.lineplot(data = paris_listings_over_time[\"price\"],color = \"red\")\n",
    "plt.xlabel(\"Years\")\n",
    "plt.ylabel(\"Average Price\")\n",
    "plt.title(\"Average Price of AirBnBs over time\")\n",
    "plt.grid(True)\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "95430cfc",
   "metadata": {
    "papermill": {
     "duration": 0.015234,
     "end_time": "2025-02-10T03:28:21.322440",
     "exception": false,
     "start_time": "2025-02-10T03:28:21.307206",
     "status": "completed"
    },
    "tags": []
   },
   "source": [
    "<div style=\"background-color: #f88379; padding: 25px; border-radius: 10px;\">\n",
    "  <h1 style=\"color: #333;\"><strong>Finding effects of Regulations on AirBnB Business</strong></h1>\n",
    "</div>"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 51,
   "id": "15f9a5d0",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2025-02-10T03:28:21.353956Z",
     "iopub.status.busy": "2025-02-10T03:28:21.353684Z",
     "iopub.status.idle": "2025-02-10T03:28:21.725758Z",
     "shell.execute_reply": "2025-02-10T03:28:21.724941Z"
    },
    "papermill": {
     "duration": 0.38949,
     "end_time": "2025-02-10T03:28:21.727268",
     "exception": false,
     "start_time": "2025-02-10T03:28:21.337778",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [
    {
     "data": {
      "image/png": "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8PDw24r177//xtvbm0ceeUS/zcrKiueff578/Hx27Nhxy2O0bt2ayMhIg8urr74KyK3FHTt2JCgoyOCx0HU/6R4LXUuYruVs165d+Pn5MWHCBI4cOUJhYSGSJLF79279sAnd8UNDQ3FxcTE4flhYGOXl5QYtcQAPPvggHh4etX6c3njjDTQaDe+9916110uSxJo1a7j33nuRJMkglvDwcHJycvT/T91zce/evfpzDQ0NJTQ0lF27dgFw6tQpsrOzDc71ZubNm1flfxAZGcnw4cMN9tu8eTOlpaXMnDkTC4trb/tTpkzB2dmZv/76C5DHhQL8+++/1XZV3oparWb//v016mavrPLroaysjIyMDNq2bYtara62u/lWr3Pd+c6YMcNgv9pOrrrvvvuqfXxfeeWVW95248aNAEybNs1ge3Wz13WeeeYZg79DQ0PJyMjQv8+tXbsWrVbLuHHjDJ5r3t7etGvXzuA9BsDGxoYnn3yyRueq06FDBzw8PAgICODpp5+mbdu2/PXXXwattDU97po1a3B3d6/2nHX/l99++w2VSsWwYcMMzqlHjx44Ojrqz6k+37sFmegyNlOff/457du3Jycnh2+//ZadO3cadPGeO3cOSZKYO3cuc+fOrfYYaWlp+Pr6cunSpWoHA7dt29aoMSckJDBv3jw2bNhQZeyHbvxRbaSnp1NYWEiHDh2qXNexY0e0Wi2XL1+mU6dO+u2VEye41sWui2fQoEE8+OCDzJ8/n48//pjBgwczZswYHn300dvuQo+LiwOujbm5nrOzs/73zMxM5s+fz88//0xaWprBfrrHKj09naKiItq1a1flWB06dODvv/+uc6yXLl2iXbt2BskByI+r7vpbcXBwICwsrNrr4uLiiImJuWESpjtnpVJJv3799AmRLkkKCQmhvLycffv24eXlRWZmpkGSFBcXx4kTJ255fJ2AgIBbnk912rRpw+OPP87y5curnUGanp5OdnY2y5cvZ/ny5TeN5c4778Te3p5du3YRHh7Orl27mD9/Pt7e3ixZsoTi4mL94xASElKj+Lp06VLt/+DHH380+Fv3/7z+tWRtbU2bNm301wcEBDBr1iw++ugjVq1aRWhoKKNHj+axxx7TJ4s3s2jRIp544gn8/f3p0aMHo0aNYsKECQZf6KpTVFTEggUL+O6770hKSkKSJP111b133Op1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\n",
      "text/plain": [
       "<Figure size 640x480 with 2 Axes>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "fig , ax = plt.subplots()\n",
    "\n",
    "ax.plot(paris_listings_over_time.index,paris_listings_over_time[\"neighbourhood\"], label = \"New Hosts\", c = \"pink\")\n",
    "\n",
    "ax.set_ylabel(\"New Hosts\")\n",
    "\n",
    "ax2 = ax.twinx()\n",
    "\n",
    "ax2.plot(paris_listings_over_time.index,paris_listings_over_time[\"price\"], label = \"Average Price\")\n",
    "\n",
    "ax2.set_ylim(0)\n",
    "\n",
    "ax2.set_ylabel(\"Average Price\")\n",
    "\n",
    "ax.set_title(\"Regulations Lead to Fewer New Hosts and Higher Prices\")\n",
    "\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "3143f218",
   "metadata": {
    "papermill": {
     "duration": 0.025661,
     "end_time": "2025-02-10T03:28:21.774918",
     "exception": false,
     "start_time": "2025-02-10T03:28:21.749257",
     "status": "completed"
    },
    "tags": []
   },
   "source": [
    "<div style=\"background-color: #f88379; padding: 25px; border-radius: 10px;\">\n",
    "  <h1 style=\"color: #333;\"><strong>Final Insights</strong></h1>\n",
    "</div>"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "43a7120a",
   "metadata": {
    "papermill": {
     "duration": 0.019885,
     "end_time": "2025-02-10T03:28:21.813230",
     "exception": false,
     "start_time": "2025-02-10T03:28:21.793345",
     "status": "completed"
    },
    "tags": []
   },
   "source": [
    "\n",
    "1. The Number of AirBnBs kept on increasing since the launch and prices kept on increasing too, due to initial traction and early adopters.\n",
    "2. After the startup is known to everyone and becomes a common utility, AirBnBs start increasing in numbers and prices also kept on decreasing.\n",
    "3. After regulation was announced around 2015 there was under confidence in the business, number of AirBnBs started decreasing and prices started increasing.\n",
    "4. Once the regulation is the new normal, during the year 2019 the number of AirBnBs have increased in number and prices kept decreasing due to more supply of them."
   ]
  },
  {
   "cell_type": "markdown",
   "id": "0e031246",
   "metadata": {
    "papermill": {
     "duration": 0.016337,
     "end_time": "2025-02-10T03:28:21.846046",
     "exception": false,
     "start_time": "2025-02-10T03:28:21.829709",
     "status": "completed"
    },
    "tags": []
   },
   "source": [
    "<div style=\"background-color: #f88379; padding: 25px; border-radius: 10px;\">\n",
    "  <h1 style=\"color: #333;\"><strong>Recommendation for AirBnB</strong></h1>\n",
    "</div>"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "cf54c412",
   "metadata": {
    "papermill": {
     "duration": 0.016204,
     "end_time": "2025-02-10T03:28:21.878621",
     "exception": false,
     "start_time": "2025-02-10T03:28:21.862417",
     "status": "completed"
    },
    "tags": []
   },
   "source": [
    "\n",
    "1. Regulations in long term rentals can impact the business adversely, there might be customer and hosts churn due to uncertainty.\n",
    "2. Such regulations might add to AirBnBs losses which might be difficult to recover later.\n",
    "3. If the customer experience is going to get impacted due to this, it would lead to incorrect brand perception.\n",
    "4. It is recommended to watch out for any such regulations at other places and be prepared for it.\n",
    "5. AirBnB can replicate such regulations at other places.\n",
    "6. They can keep strict rules to onboard and release the hosts.\n",
    "7. They can limit the number of AirBnBs in a locality to ensure the public has enough rental options and the government doesn't step in."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "id": "f44e0b7c",
   "metadata": {
    "papermill": {
     "duration": 0.016198,
     "end_time": "2025-02-10T03:28:21.911028",
     "exception": false,
     "start_time": "2025-02-10T03:28:21.894830",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [],
   "source": []
  }
 ],
 "metadata": {
  "kaggle": {
   "accelerator": "gpu",
   "dataSources": [
    {
     "datasetId": 1530930,
     "sourceId": 2526818,
     "sourceType": "datasetVersion"
    }
   ],
   "dockerImageVersionId": 30840,
   "isGpuEnabled": true,
   "isInternetEnabled": true,
   "language": "python",
   "sourceType": "notebook"
  },
  "kernelspec": {
   "display_name": "Python 3",
   "language": "python",
   "name": "python3"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.10.12"
  },
  "papermill": {
   "default_parameters": {},
   "duration": 17.045731,
   "end_time": "2025-02-10T03:28:22.445156",
   "environment_variables": {},
   "exception": null,
   "input_path": "__notebook__.ipynb",
   "output_path": "__notebook__.ipynb",
   "parameters": {},
   "start_time": "2025-02-10T03:28:05.399425",
   "version": "2.6.0"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 5
}
