<
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 5px;
}

.bottom-nav button span {
  font-size: 21px;
}

.bottom-nav .active {
  color: white;
}

@media (max-width: 600px) {
  .series-grid {
    grid-template-columns: repeat(3, 1fr);
  }

  .card {
    height: 170px;
  }

  .hero h1 {
    font-size: 43px;
  }
}
</style>